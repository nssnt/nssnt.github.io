---
layout: post
title:  "Live Wallpapers Questions"
categories: Wallpapers
tags:  wallpapers live question backend requirement question
author: Leo
---
Dicuss what is needed and missing in our app, and looking for a balance solution on cost and quality.




* content
{:toc}    




## Backend
- Situation
Current backend just simply responding for requesting content from frontend, and distribute the content.
	- No categorise function (tags)
		- Example, [Live Wallpapers for Me from Apalon](https://itunes.apple.com/app/id1069361548); [Live Wallpapers Now](https://itunes.apple.com/app/id1098805023)
	- Cannot differentiate the wallpapers size for different sizes of device screen.
		- iPhone 6s/6s+, 7/7++ and 8/8+
		- iPhone X


- Question:
	- Do we need to categorise the image?
		- From backend or frontend?
			- Backend: flexible, can do live adjustment
				- Needs backend programmer
				- Needs to design backend logic
				- Time Consuming
		- Frontend: change for image categories require binary update
			- No backend required
			- Logic on the frontend, but not flexible in future update and adjustment of content.
		- Through firebase: frontend read configuration file from firebase first, and then request wallpapers according to the file.
			- Issue: not yet sure how the backend logic structured, and whether it is allowed  to do so
			- Needs backend programmer to cooperate
	- Optimization for screen sizes? (Competitor research required)
		- Are we going to optimize for different screen sizes?
			- If going to optimize the app for all screen sizes, detailed methods need research
				- Backend?
				- Frontend?
					- There is solution from frontend, but may lower down the content quality.
					- Needs unifying the content size.
						- Reuse of the old resources will require continue using of 1080P resources, low quality on iPhone X
						- Use new 1125P resources will require prepare new resources and waste old resources
						- Combine use of resources will increase the workload both from backend and frontend
## Frontend (APP)
- Situation
For now the app is just directly showing the live wallpapers one by one through scrolling the screen, no extra functions.
	- Scroll the view different wallpapers
	- Live wallpapers directly playing after successfully loaded
	- Download wallpapers to gallery
- Questions:
	- As the live wallpapers size is large, while user browsing the wallpapers, the cache data increase very rapidly, do we need to add 鈥淐lear Cache Data鈥?
	- Add Push Notification?
	- Reduce Motion? - a feature to reduce the frames of the live wallpapers preview in app to reduce the cellular data consuming. Apalon Live Wallpapers for Me allow user to set limitation on cache size, once the data reaches the limitation the cached data will be removed.
	- Wallpapers categorization?
	- Needs thumbnails for live wallpapers to create a gallery view like our other wallpapers? Or Just directly go to large image view?
		- For now, no popular live wallpapers use gallery view for live wallpaper apps.
	- Share? Example, [Live Wallpapers Now](https://itunes.apple.com/app/id1098805023) which is the one with most user reviews.
	- Like (Self collection)? Example, [Live Wallpapers Now](https://itunes.apple.com/app/id1098805023) which is the one with most user reviews.
	- Report of content?
- Content
	- Situation
		- Content stored on server side
		- Content has no tags
		- All content are 1080P resources
		- No productive content creator, which caused the production speed of content is very slow
	- Question:
		- If going to perfectly optimize content for iPhone X?
		- Content create:
			- Create content through making little animations?
				- Require art resources
					- Not sure whether our artists can handle?
					- Lack of creativity
			- Create content through transferring videos?
				- Require to create a content creator
				- Will be comparably faster in mass production
				- Wider choices of content
## Monetization:
- Situation:
	- Banner Ads
	- Interstitial Ads
- Questions?
	- As no thumbnail list, do we need rectangle in between the large images?
	- Popular live wallpapers provide premium membership to access all further and existing content with no ads; or video ads to unlock certain content.
		- Do we need to add video ads?
		- Or subscription for premium content?
		- Or direct purchase through IAP?
