---
layout: post
title:  "HealthKit Deep Research"
categories: Research iOS
tags:  research ios healthkit apple sample
author: Leo
---

# iOS HealthKit Deep Research
1/24/2018 3:16:37 PM 
## Introduction
HealthKit APIs had been introduced from iOS 8.0 and WatchOS 2.0 or later.
**Build Requirement:** iOS 8 SDK and Xcode 8
**Runtime Requirement:** iOS 8 SDK

* content
{:toc}

Healthkit is used to manage data from a wide range of sources, automatically merging the data from difference sources based on users' preferences, and Apps can also access the raw data for each sources, merging them in their own way.

As health data can be sensitive to users, Healthkit grants the users the permission control over all the information that they want and do not want to share to other apps, meanwhile the user can explicitly grant each app the permission to read and write data to the Healthkit store and user can grant or deny permission separately for each type of data. For example, the user could let the app to read the weight, but prevent it from reading the age. 
> **Note:** To prevent the leaking of information, the Apps do not know when an access of data has been denied by the users, in this way that this type of data will return as no data exist.

The HealthKit data saved locally on user's device. For security purpose, the HealthKit store is encrypted when the devices is locked, and can only be accessed by an authorized app.For this reason, the app may not be able to read data from the HealthKit store when it is running in the background, but still can write data to the store, even when the phone is locked. The data will be cached and saves to the store as soon as the phone is unlocked.
> **Important:**
> -
> * HealthKit API can only be accesses when your app is designed to provide health and fitness services;
> * **App's role as a health and fitness service must be clear in both marketing text and user interfaces**, the following guidelines must be applied to the app:
>  * Your app may not use information gained through the use of the HealthKit framework for advertising or similar services. Note that you may still serve advertising in an app that uses the HealthKit framework, but you cannot use data from the HealthKit store to serve ads;
>  * You must not disclose any information gained through HealthKit to a third party without express permission from the user. Even with permission, you can only share information to a third party if they are also providing a health or fitness service to the user.
>  * You cannot sell information gained through HealthKit to advertising platforms, data brokers, or information resellers.
>  * If the user consents, you may share his or her HealthKit data with a third party for medical research.
>  * You must clearly disclose to the user how you and your app will use their HealthKit data.
>  
> Other requirements: [Apple Review Guide](https://developer.apple.com/app-store/review/guidelines/#health-and-health-research)

## Implementation
### Get Authorization
* Ensure the Healthkit been switched on in the "Capabilities". Things needs to be noted here is to make sure that if the app needs to support devices running lower than iOS 8.0, it will needed to remove the value of <code>healthkit</code> in <code>Required device capabilities</code> from <code>info.plist</code>, or the app cannot running when installed on the device lower than iOS 8.0.
![](http://www.pluto-y.com/content/images/2015/12/2.png)
* Meanwhile, as the HealthKit only works on iPhone, it is not usable on iPad or iPod, so check availability is 
	```objc
	if(NSClassFromString(@"HKHealthStore") && [HKHealthStore isHealthDataAvailable])  
	{
	   // Add your HealthKit code here
	}
	```
* Request Authorization, since the HealthKit stores lots of user sensitive data, so if the app needs to access to the HealthKit data, it requires the users to grant the permissions. The permission both includes read and write. It could directly use ``` requestAuthorizationToShareTypes: readTypes: completion:```
	```objc
	HKHealthStore *healthStore = [[HKHealthStore alloc] init];
	
	// Share body mass, height and body mass index
	NSSet *shareObjectTypes = [NSSet setWithObjects:
	                           [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMass],
	                           [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierHeight],
	                           [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMassIndex],
	                           nil];
	
	// Read date of birth, biological sex and step count
	NSSet *readObjectTypes  = [NSSet setWithObjects:
	                           [HKObjectType characteristicTypeForIdentifier:HKCharacteristicTypeIdentifierDateOfBirth],
	                           [HKObjectType characteristicTypeForIdentifier:HKCharacteristicTypeIdentifierBiologicalSex],
	                           [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierStepCount],
	                           nil];
	
	// Request access
	[healthStore requestAuthorizationToShareTypes:shareObjectTypes
	                                    readTypes:readObjectTypes
	                                   completion:^(BOOL success, NSError *error) {
	
	                                       if(success == YES)
	                                       {
	                                           // ...
	                                       }
	                                       else
	                                       {
	                                           // Determine if it was an error or if the
	                                           // user just canceld the authorization request
	                                       }
	
	                                   }];
	```
### Read/Write Data
#### Data Types
* There are two main data types, **characteristics** and **samples**. Characteristics, such as the user's birth date or blood type, usually don't change. Sample represent data at a particular point in time.
* **Quantity samples** are the most common data types. They include the user's height and weight, steps taken, the user's temperature, pulse rate, etc.
* **Workouts**, which belong to the samples category, are intended specifically for representing runs, walks, rides, etc.
* For more information, please check HealthKit Framework references: [LINK](https://developer.apple.com/documentation/healthkit/hkhealthstore?language=objc) 
#### Methods
* ```sharedManager``` is a class method that creates the singleton object the first time it is called, and returns that instance each time the method is invoked. The ```dispatch_once``` function is a GCD (Grand Central Dispatch) function that guarantees that the block passed to it is called only once, even if the ```sharedManager``` method would get called from multiple threads at the same time.
* ```requestAuthorization``` is a method that asks the HealthKit store for permissions to read and/or write the specific data that we need. You need to call this method before you use any of the writing/reading APIs of the ```HKHealthStore``` class. In case the user denies some (or all) permissions, HealthKit won't inform you about that. The fact that the user doesn't want to share some types of data is information in itself. That's how much Apple cares about privacy.
* The ```readBirthDate``` method returns the user's birth date. It return ```nil``` if there was an error or if the user hasn't entered a birth date.
* ```writeWeightSample:``` saves a weight measurement to HealthKit. I've commented the code so you should have a general idea of what's going on in the method. Once we have the ```HKQuantitySample``` object, we save it to the ```HKHealthStore``` instance, using its ```saveObject:withCompletion:``` method. This method is used for every type of health data and we will also use it in the second part of this tutorial when saving workouts.
#### Classes
* ```HKHealthStore``` This is your window to HealthKit data. Apple recommends using just one instance of this class in your app and that lends itself to the singleton pattern really well. You use it for asking the user for permissions, saving samples and/or workouts to HealthKit, and querying the stored data. These are just a few of the tasks of the ```HKHealthStore``` class.
* ```HKUnit``` Instances of this class can represent either basic units, such as meters, seconds, and grams, or complex units that are created by combining basic units, such as km/h or or g/m鲁. Complex units can be conveniently created from strings.
* ```HKQuantity``` Instances of this class store a value (represented by double) for a given unit (represented by ```HKUnit```). You can use the ```doubleValueForUnit:``` method to convert the quantity's value to the unit that's passed in. An example of such a conversion would be creating distance quantity with unit of meters and asking for its value in feet.
* ```HKQuantityType``` HealthKit uses quantity types to create samples that store a numerical value. It is recommended to use ```quantityTypeForIdentifier:``` when creating quantity types. A few examples of quantity types are cycling distance, energy burned, steps, and flights climbed.
* ```HKQuantitySample``` An instance of this class represents a sample that has a quantity type (represented by ```HKQuantityType```), a quantity (represented by ```HKQuantity```), and a start and end date. In case the sample doesn't span over a period of time, the end date can be the same as the start date.
#### Sample Code
* Read the user's birth date
* Write a weight sample to HealthKit
* Write a workout to HealthKit
  ```objc
	#import <UIKit/UIKit.h>
 
	@interface GSHealthKitManager : NSObject
 
	+ (GSHealthKitManager *)sharedManager;
 
	- (void)requestAuthorization;
 
	- (NSDate *)readBirthDate;
	- (void)writeWeightSample:(CGFloat)weight;
 
	@end
  ```  
	```objc
	#import "GSHealthKitManager.h"
	#import <HealthKit/HealthKit.h>
	 
	 
	@interface GSHealthKitManager ()
	 
	@property (nonatomic, retain) HKHealthStore *healthStore;
	 
	@end
	 
	 
	@implementation GSHealthKitManager
	 
	+ (GSHealthKitManager *)sharedManager {
	    static dispatch_once_t pred = 0;
	    static GSHealthKitManager *instance = nil;
	    dispatch_once(&pred, ^{
	        instance = [[GSHealthKitManager alloc] init];
	        instance.healthStore = [[HKHealthStore alloc] init];
	    });
	    return instance;
	}
	 
	- (void)requestAuthorization {
	     
	    if ([HKHealthStore isHealthDataAvailable] == NO) {
	        // If our device doesn't support HealthKit -> return.
	        return;
	    }
	     
	    NSArray *readTypes = @[[HKObjectType characteristicTypeForIdentifier:HKCharacteristicTypeIdentifierDateOfBirth]];
	     
	    NSArray *writeTypes = @[[HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMass]];
	     
	    [self.healthStore requestAuthorizationToShareTypes:[NSSet setWithArray:readTypes]
	                                             readTypes:[NSSet setWithArray:writeTypes] completion:nil];
	}
	 
	- (NSDate *)readBirthDate {
	    NSError *error;
	    NSDate *dateOfBirth = [self.healthStore dateOfBirthWithError:&error];   // Convenience method of HKHealthStore to get date of birth directly.
	     
	    if (!dateOfBirth) {
	        NSLog(@"Either an error occured fetching the user's age information or none has been stored yet. In your app, try to handle this gracefully.");
	    }
	     
	    return dateOfBirth;
	}
	 
	- (void)writeWeightSample:(CGFloat)weight {
	     
	    // Each quantity consists of a value and a unit.
	    HKUnit *kilogramUnit = [HKUnit gramUnitWithMetricPrefix:HKMetricPrefixKilo];
	    HKQuantity *weightQuantity = [HKQuantity quantityWithUnit:kilogramUnit doubleValue:weight];
	     
	    HKQuantityType *weightType = [HKQuantityType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMass];
	    NSDate *now = [NSDate date];
	     
	    // For every sample, we need a sample type, quantity and a date.
	    HKQuantitySample *weightSample = [HKQuantitySample quantitySampleWithType:weightType quantity:weightQuantity startDate:now endDate:now];
	     
	    [self.healthStore saveObject:weightSample withCompletion:^(BOOL success, NSError *error) {
	        if (!success) {
	            NSLog(@"Error while saving weight (%f) to Health Store: %@.", weight, error);
	        }
	    }];
	}
	 
	@end
  ```
* Adding a Workout
	* Extend the ```GSHealtKitManager``` class
	Add the following method declaration to **GSHealthKitManager.h**
	```objc
 	- (void)writeWorkoutDataFromModelObject:(id)workoutModelObject;
 	```
	* Add a new share type to the ```requestAuthorizationToShareTypes:readTypes:completion:``` method by adding the workout type to the ```writeTypes``` array.
	```objc
	NSArray *writeTypes = @[[HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMass],
                        [HKObjectType workoutType]];
	```
	* Add the ```writeWorkoutDataFromModelObject:``` method at the bottom of the ```GSHealthKitManager``` class.
	```objc
	- (void)writeWorkoutDataFromModelObject:(id)workoutModelObject {
     
    // In a real world app, you would pass in a model object representing your workout data, and you would pull the relevant data here and pass it to the HealthKit workout method.
     
    // For the sake of simplicity of this example, we will just set arbitrary data.
    NSDate *startDate = [NSDate date];
    NSDate *endDate = [startDate dateByAddingTimeInterval:60 * 60 * 2];
    NSTimeInterval duration = [endDate timeIntervalSinceDate:startDate];
    CGFloat distanceInMeters = 57000.;
     
    HKQuantity *distanceQuantity = [HKQuantity quantityWithUnit:[HKUnit meterUnit] doubleValue:(double)distanceInMeters];
     
    HKWorkout *workout = [HKWorkout workoutWithActivityType:HKWorkoutActivityTypeRunning
                                                  startDate:startDate
                                                    endDate:endDate
                                                   duration:duration
                                          totalEnergyBurned:nil
                                              totalDistance:distanceQuantity
                                                   metadata:nil];
     
    [self.healthStore saveObject:workout withCompletion:^(BOOL success, NSError *error) {
        NSLog(@"Saving workout to healthStore - success: %@", success ? @"YES" : @"NO");
        if (error != nil) {
            NSLog(@"error: %@", error);
        }
    }];
	}
	```

>References:
> [HealthKit Framework Reference](https://developer.apple.com/library/ios/documentation/HealthKit/Reference/HealthKit_Framework/)
> [Fit: Store and Retrieve HealthKit Data](https://developer.apple.com/library/ios/samplecode/Fit/Introduction/Intro.html)
> [Apple Store Review Guide](https://developer.apple.com/app-store/review/guidelines/#healthkit)
> [envatotut+: Getting Started with HealthKit: Part 1](https://code.tutsplus.com/tutorials/getting-started-with-healthkit-part-1--cms-24477https://code.tutsplus.com/tutorials/getting-started-with-healthkit-part-1--cms-24477)
> [envatotut+: Getting Started with HealthKit: Part 2](https://code.tutsplus.com/tutorials/getting-started-with-healthkit-part-2--cms-24484)
> [CSDN Blog: HealthKit妗嗘灦鐨勭畝瑕佷笌鍩烘湰浣跨敤锛圤C鐗堬級](http://blog.csdn.net/qq_31292239/article/details/52023519)
