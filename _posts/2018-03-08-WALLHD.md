---
layout: post
title:  "Wallpapers App Improve methods"
date:   2018-03-8 11:40:18 +0800
categories: Project Docs
tags: projects wallpapers design prd
author: Leo
mathjax: true
---

# Ways to Improve WALLHD line 
1/23/2018 5:21:47 PM 
</br>
## Zedge
As the most recent update of the Zedge Wallpapers & Ringtones, the ranking of the app has a significate increasement in its category ranking and overall ranking.
</br>
<ul>
	<li>Transfer the focus on normal active collection of content to professional artist provided content.
		<ul>
			<li>Advantages:
				<ul>
					<li>Reduced the work amount of the active collection of content and editing cost
					<li>Reduce the copyright content risk
					<li>Professional content ensured the content quality
					<li>Content becomes unique
					<li>App looks much more professional
					<li>Increased the user participating on the content
				</ul>
			<li>Disadvantages:
				<ul>
					<li>As to cooperate with the professional artists, extra time and cost will be introduced to hire this professionals
					<li>Share profit with the professionals
					<li>Quanity and of the content will decrease, as well as the update frequency
					<li>Content from same professionals will tends to be very similiar
				</ul>
		</ul>
	<li>Improved UI and UX Design
		<ul>
			<li>Reduced the sections on the bottom tabbar from 5 to 4, which help user easier to choose section from many to less.
			<li>Enhaced the content UI elements
				<ul>
					<li> Reduced the button on the bottom tabbar from 5 to 4 to make the buttom more clear to users on its purpose
					<li>Put many of the category information to the content screen, which make it easier for user to read and understand visually.
						<ul>
							<li>Indicate the daily new images at the top of the screen which give users a clear vision on new items
							<li>Promotion space where to hightlight the features or collections that wanted to promote
							<li>Separate the normal content from above, which is a full list of everything
						</ul>
					<li>Introduce a new big image preview method in professional artwork section
						<ul>
							<li>Big enough for users to view the full images
							<li>Enough space to place images info
							<li>Consume less system resources to improve the app performace
						</ul>
					<li>Hide more action buttons in the action sheet, which make the app UI much more clean		
				</ul>
		</ul>
	<li>Monetization
		<ul>
			<li>Removed banner ads from the app, which increased the app UI quality, banner is the most annoying ads in the image based app as it will cover parts of the images
			<li>within the gallery list, replaced part of the rectange/native ads for promotion purpose, it could be either in-app promotion or promotion of other apps
			<li>Fullscreen ads is still availiable, but no longer existi while browing images
			<li>Native ads in between the fullscreen image preview which is less interrupting
			<li>Introduced Zedge Credit for premium content, gain through watch video ads.
				<ul>
					<li>The premimum content unlock by using Zedge Credit, the user could gain the credit by watching the ads ahead for further use
					<li>The user could also unlock the content by directly watching the video ads.
				</ul>
		</ul>
	<li>Other features
		<ul>
			<li>Zedge start selling relevant image based real product, such as T-shirt and Phone case
			<li>Ringtones, includes ringtones and notification sound, such content is provided through professional channels, where to protect the app from the copyright problems, Zedge did not provided them directly
			<li>Live Wallpapers which is very popular right now in the market.
		</ul>
</ul>
<br>
## Our products
**TICK092 - 10,000+ Wallpapers** and **TICK2005 - Girly Wallpapers** are two of our major apps targeting different users, TICK2005 is more specifically targeting the female market where TICK092 is more generic. Both of the two apps are based on very similiar framework in code and UI, with very little differences.
<br>
The whole framework is very similar to the Zedge Wallpapers before version 3.0.0 contains following secions:
<ul>
	<li>Gallery List
		<ul>
			<li>Daily, where have all the latest images at the top of the list
			<li>Hot, where contains the most popular images sequenced based on the number of likes from users and the date that the images uploaded.
			<li>Collections, where is sets of high quality hand-picked images with similar themes or participate purpose
			<li>Liked, where is a space to store the images that user used to like
			<li>Categories, which is very similar to Collections, but not hand picked, just based on the images' nature tags.
		</ul>
	<li>Search feature, based on the images tags, fuzzy search is interested by users, but we are not be able to achieve this on tech side in short time
	<li>User upload, user could upload images to our server, but not directly interaction with our app which is user perferred
	<li>Change skin, which help us more easier to interact with the events themes on the UI side
	<li>Monetization
		<ul>
			<li>Banner ads while browsing the full images
			<li>Interstitial ads while browsing the full images
			<li>Rectangle ads in the gallery list or in between the large images
		</ul>
</ul>
As what we can see here, our apps is very targeting on the content itself, which is very focus, and looks very successful so far. But as all the content is managing by ourselves, which is a very heavy workload as long as the users wants more, and also very risky on the content copyright. Meanwhile, the staff who works on the content needs to very clear on the current market trends and familiar with the current user habits, so if replace any of the content staff will be very risk.
<br>
And also, there are some other issues we can forecast:
<ul>
	<li>Image size, as we are targeting both iOS and Android platform, our app is facing lots of screen sizes, our backend needs to be always maintained to cut different image sizes for different screens, and once there is one more new popular screen ratio, we need to add extra patch to the server which makes the server becomes ugly and bloated to manage.
		<ul>
			<li>As some of the apps, the just provided full sized images, which is mostly a PC screen high resolution images, and let user decided to cut or reside the image to best fit their screen, so that we just need to prepare one image source, and reduce the server workload, meanwhile it also reduced the logic on determine the screen size on the frontend.
		</ul> 
	<li>Monotonous content type, just images, which is good and success, but as long as the user get bored to the image types we have, we will lost them once they find anything more interesting in other apps, and there are many substitute products in the store, and we have nothing unique.
		<ul>
			<li>Users using our apps are who interested in using images as wallpapers for the phones, which means that this group of users interested in customized their phones, so which give us some ideas that they may also like to customize other parts of their phones, like:
				<ul>
					<li>Ringtones
					<li>Notification tones
					<li>Alarm sound
					<li>App icon
					<li>Navigation bar (Android)
					<li>Keyboard skin
				</ul>
		</ul>
</ul>
<br>
## Improvements we can do
### Content
### Current content
- We have huge amount of content storage, which is about 300,000 images and keep increasing. This is our advantages, and we do not want to lose it, but enhance it, so it is better that we keep working on it, but consider the copyright issues, we should move more towards on the generic images, rather than the very trended images from anime, movies, fashion people and brands.
<br>
- Coorperate with Professional will be very interested in to have, but it will be very difficult to manage both on working and finacial aspects. Instead of this, we should enhance our user upload features to improve the user participation in our app, which to make more user content visable in our app. And categories the images for the premium users.
<br>
- Reorganize the new and hot images to make them eaiser to be found by users
### Ehrich Content
Here more content is not means to have more images, but more content types. As the extra content is purely a different section of the app, so the logic will not affect the current apps, but an extra feature to app aside from the main framework which is not very risky
<br>
<ul>
	<li><strong>Ringtones</strong>
		<ul>
			<li>Ringtones could be gain through some website like the image content we get, it could be get for free or purchase from some sound dealers.
		</ul>
	<li><strong>Notification</strong>
		<ul>
			<li>Same as the ringtones, but much eaiser to get.
		</ul>
	<li><strong>Alarm</strong>
		<ul>
			<li>the same, but need to choose those which is appropirate to wake up people, not too noisy, nor too loud.
		</ul>
</ul>
As our current apps performance is very stable, enrich of the content type will not affect the user too much, in the extreme situation is they just dislike the new content type we provide, but the app is still performing the image part well, and at least it will help us understand which kinds of the content the user perferred.
### Feature
#### Image Editor
As many of the users of our app will purpose to use our images for social sharing, so humor and quotes images are always asked by the users, but as we are only providing standardized size images, which is not very appropriate for the humor content, so we will be more focused on the quotes.
<br>
But preparing quotes content is not just find images with quotes on the internet, but need a lot of image editing work, and not easy to catch up everyone's demand, such as sometimes user just want us to put their baby's name on the wallpapers, so that we can provide some image editor tools to allow the users to use any of the wallpapers to create any types of quotes wallpapers on their own, and we can also see many such independent apps performing well in the market, and it is not difficult to achieve on tech side
#### Image Motion
Which is part of the image editor, but just separate out from the preivous editor to make it much easier to use to help user to create their own unique wallpapers, by using filter and simple adjustment of the colors.
#### User Profile
Which is an old topic, it is an implement of our app to improve the user participation, which allows user to generate their own content and load to our resource pool, and it could reduce our pressure from content generate, and even when the user generated content reaches certain amount, we could completely get off from the content part, but focus on the tech side to improve the app performance.
