---
date: 2022-06-20
title: Scene Analytics
description: Track player analytics for your scene
categories:
  - development-guide
type: Document
---

For advanced builds, experiments, and events on Decentraland - it can be helpful to have a look under the hood at what's really transpiring on a scene. As a contribution to the [Decentraland DAO](https://forum.decentraland.org/t/dao-qmdxcqc-wemeta-builder-tag/8194), a team of engineers - WeMeta - helped create a tool to do just that. Through a one-line code snippet (the 'Builder Tag'), you can expose/access (via API or dashboard) a myriad of interesting scene data including unique visitors, heat maps of visitor foot traffic, visitor activity, your scene rank and more. 


If you would like step-by-step instructions for installation, please feel free to watch the video below:

<p align="center">
<a href="https://www.youtube.com/watch?v=QuO4qhYo01c" target="_blank">
<img src="https://img.youtube.com/vi/QuO4qhYo01c/0.jpg">
</a>
</p>



The first step to acquiring a Builder Tag is to create a [scene management account here]("https://analytics.wemeta.world"). Just click "Sign In" on the bottom-left sidebar, and follow the prompts. This account allows you to manage your scene data as well as the Builder Tags you create.

Next, there are two main methods by which to create Builder Tags and access scene data.

- [Via the WeMeta UI](#Via-the-WeMeta-UI)
- [Via the API](#Via-the-API) 



## Via the WeMeta UI

First, [log-in to the WeMeta partner portal here]("https://analytics.wemeta.world") using the account you created above.

Once logged in, you can create new Decentraland Scenes by clicking the plus button on the sidebar:


<img src="https://files.readme.io/25f12f5-Create_New_Scene.png">

After clicking the plus button, you will enter the scene creation dialogue. Here you'll be prompted to provide a friendly label for your new scene, and given a Builder Tag. This Builder Tag is the code snippet that will allow you to collect & monitor analytics on your scene.



<p align="center">
<img src="https://files.readme.io/9051c91-Scene_Tag.png" width="40%"/>
</p>

## Implement tag in scene

To implement the Builder Tag, go into the code of your Decentraland Scene and paste the snippet at the top of your `game.ts` file. Next, install the [@wemeta/analytics]("https://www.npmjs.com/package/@wemeta/analytics") package via npm.  Finally, go to your `tsconfig.json` and add `"moduleResolution":"node"` under `compilerOptions`. 

You can now deploy your scene and kickstart the data collection!

*Builder Tag Code Snippet*

```ts
import { WeMeta } from "@wemeta/analytics"
new WeMeta('[your unique scene-id]');
```

An example of this implementation can also be found on the [repo here]("https://github.com/wemeta-labs/wemeta-tower-dcl-scene").

Once your Builder Tag has been implemented and your scene deployed, a dashboard will automatically be created for your scene data. New scenes that you create will show up on the sidebar listed under the friendly name that you supply in the setup process.

Data will begin to flow to the dashboards within ~10 minutes. If desired, you can also access your data programmatically via API per the instructions in the next section.

<img src="https://files.readme.io/5bc3ea4-Scene_Dashboard.png">




## Via the API

After creating an account, you may [request an API key](https://wemeta.readme.io/docs/api-key-request) which should be granted within ~1 day. This key ensures that only you can programmatically create, access, and manage your own scenes, so please guard it well.

There are several ways to make requests of the API. We suggest leveraging either Python or cURL.

#### Issuing API calls via Shell


```ts
curl --request POST \
     --url https://api.wemeta.world/api/v1/public_api/scene_management/endpoint \
     --header 'Accept: application/json' \
     --header 'Content-Type: application/json' \
     --header 'x-api-key: <your API key>' \
     --data '
{
     "body_parameter": "parameter",
}
'
```

#### Issuing API calls via Python

```ts
import requests

url = "https://api.wemeta.world/api/v1/public_api/scene_management/endpoint"

payload = {
    "body_parameter": "parameter",
}
headers = {
    "Accept": "application/json",
    "Content-Type": "application/json",
    "x-api-key": "<your API key>"
}

response = requests.post(url, json=payload, headers=headers)

print(response.text)
```

Once you've received your API key, you can make use of the following commands to create Builder Tags and view scene data. What's more, everything you create will also be created on your dashboard! You can feel free to leverage either method interchangeably.

> Note: Please note that for scene management & scene data calls, your supplied API key must match the key associated with your account email.


#### API Index

**Scene Management**

- [Create a scene](#[POST]-Create-a-scene)
- [View builder tags](#[Get]-View-builder-tags)

**Foot Traffic**

- [Retrieve foot traffic by parcel](#[Get]-Retrieve-foot-traffic-by-parcel)
- [Retrieve active visitors & scene rank](#[Get]-Retrieve-active-visitors-and-scene-rank)
- [Retrieve active users by parcel](#[POST]-Retrieve-active-users-by-parcel)

**Session**

- [Retrieve visit times](#[Get]-Retrieve-visit-times)
- [Retrieve number of visits per visitor](#[Get]-Retrieve-number-of-visits-per-visitor)

**Visits**

- [Retrieve unique visitor count](#[Get]-Retrieve-unique-visitor-count)

**Wearables**

- [Retrieve wearables on scene](#[Get]-Retrieve-wearables-on-scene)


#### [POST] Create a scene

Use the following endpoint to create Builder Tags programmatically. To implement the created Builder Tags, please visit the [installation instructions above](#Implement-tag-in-scene)

**Body Params**
- `email (string)`: WeMeta Account email
- `scene_friendly_name (string)`: Friendly scene name/title

```ts

curl --request POST \
     --url https://api.wemeta.world/api/v1/public_api/scene_management/create_scene_tag \
     --header 'Accept: application/json' \
     --header 'Content-Type: application/json' \
     --header 'x-api-key: <your api key>' \
     --data '
{
     "email": "your email",
     "scene_friendly_name": "friendly name"
}
'
```


#### [Get] View builder tags

Use the following endpoint to access your created Builder Tags. Builder Tags will be denoted by their string as well as by the friendly name supplied in the creation process. To implement the created Builder Tags, please visit the [installation instructions above](#Implement-tag-in-scene)

**Body Params**
- `email (string)`: WeMeta Account email

```ts
curl --request GET \
     --url 'https://api.wemeta.world/api/v1/public_api/scene_management/view_scenes?email=email' \
     --header 'Accept: application/json' \
     --header 'x-api-key: <your api key>'
```


#### [Get] Retrieve foot traffic by parcel

Returns the foot traffic by parcel for a specific scene with multiple time granularities.

**Query Params**
- `scene_tag (string)`: The ID from your Builder Tag

[Sample Response](https://github.com/wemeta-labs/builder-tag-docs/blob/main/docs/responses/foottraffic_by_parcel.json)

```ts
curl --request GET \
     --url 'https://api.wemeta.world/api/v1/public_api/foottraffic_by_parcel?scene_tag=sceneId' \
     --header 'Accept: application/json' \
     --header 'x-api-key: <your api key>'
```



#### [Get] Retrieve active visitors and scene rank

Returns the number of active visitors of a specific parcel coordinate. If start_time is specified, the response will be the number of active visitors between the start_time and now.

**Query Params**
- `parcel_coord (string)`: Parcel coordinates for the query (e.g. -132,90)
- `start_time (int)`: Default is one day ago

[Sample Response](https://github.com/wemeta-labs/builder-tag-docs/blob/main/docs/responses/active_visitor_count.json)

```ts
curl --request GET \
     --url 'https://api.wemeta.world/api/v1/public_api/active_visitor_count?parcel_coord=parcels' \
     --header 'Accept: application/json' \
     --header 'x-api-key: <your api key>'
```



#### [Post] Retrieve active users by parcel

Returns the current Foot Traffic of specified parcels from Decentraland's catalyst server.

**Body Params**
- `parcels (string)`: List of parcel coordinates for the query (e.g. ['0,0', '0,1', '0,2'])

[Sample Response](https://github.com/wemeta-labs/builder-tag-docs/blob/main/docs/responses/parcels_foottraffic_catalyst.json)

```ts
curl --request POST \
     --url https://api.wemeta.world/api/v1/public_api/parcels_foottraffic_catalyst \
     --header 'Accept: application/json' \
     --header 'Content-Type: application/json' \
     --header 'x-api-key: <your api key>' \
     --data '
{
     "parcels": "["0,0","0,1","0,2"]"
}
'
```


#### [Get] Retrieve visit times

Returns multiple variations of visit time of a specific scene with average, total, median and histograms for total time, total active time, and total idle time with multiple time granularities.

**Query Params**
- `scene_tag (string)`: The ID from your Builder Tag

[Sample Response](https://github.com/wemeta-labs/builder-tag-docs/blob/main/docs/responses/visit_time.json)

```ts
curl --request GET \
     --url 'https://api.wemeta.world/api/v1/public_api/visit_time?scene_tag=sceneId' \
     --header 'Accept: application/json' \
     --header 'x-api-key: <your api key>'
```


#### Retrieve number of visits per visitor

Returns the number of sessions per visitor for a specific scene with average, median, and histograms for the number of sessions per visitor with multiple time granularities.

**Query Params**
- `scene_tag (string)`: The ID from your Builder Tag

[Sample Response](https://github.com/wemeta-labs/builder-tag-docs/blob/main/docs/responses/number_of_sessions_per_visitor.json)

```ts
curl --request GET \
     --url 'https://api.wemeta.world/api/v1/public_api/number_of_sessions_per_visitor?scene_tag=sceneId' \
     --header 'Accept: application/json' \
     --header 'x-api-key: <your api key>'
```



#### Retrieve unique visitor count

Returns the number of unique visitors of a specific scene with multiple time granularities.

**Query Params**
- `scene_tag (string)`: The ID from your Builder Tag

[Sample Response](https://github.com/wemeta-labs/builder-tag-docs/blob/main/docs/responses/unique_visitor_count.json) 

```ts
curl --request GET \
     --url 'https://api.wemeta.world/api/v1/public_api/unique_visitor_count?scene_tag=sceneId' \
     --header 'Accept: application/json' \
     --header 'x-api-key: <your api key>'
```


#### Retrieve wearables on scene

Returns the wearables used in a specific scene with the number of players and percentage of players using the wearable with multiple time granularities.

**Query Params**
- `scene_tag (string)`: The ID from your Builder Tag

[Sample Response](https://github.com/wemeta-labs/builder-tag-docs/blob/main/docs/responses/wearables.json)

```ts
curl --request GET \
     --url 'https://api.wemeta.world/api/v1/public_api/wearables?scene_tag=sceneId' \
     --header 'Accept: application/json' \
     --header 'x-api-key: <your api key>'
```


> Note: For additional help or suggestions you can reach out to the team [here](https://docs.wemeta.world) or [here](mailto:contact@wemeta.world)


