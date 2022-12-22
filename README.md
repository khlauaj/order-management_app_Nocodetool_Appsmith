![](https://raw.githubusercontent.com/appsmithorg/appsmith/release/static/appsmith_logo_primary.png)

This app is built using Appsmith. Turn any datasource into an internal app in minutes. Appsmith lets you drag-and-drop components to build dashboards, write logic with JavaScript objects and connect to any API, database or GraphQL source.

![](https://raw.githubusercontent.com/appsmithorg/appsmith/release/static/images/integrations.png)

### [Github](https://github.com/appsmithorg/appsmith) • [Docs](https://docs.appsmith.com/?utm_source=github&utm_medium=social&utm_content=appsmith_docs&utm_campaign=null&utm_term=appsmith_docs) • [Community](https://community.appsmith.com/) • [Tutorials](https://github.com/appsmithorg/appsmith/tree/update/readme#tutorials) • [Youtube](https://www.youtube.com/appsmith) • [Discord](https://discord.gg/rBTTVJp)

##### You can visit the application using the below link

###### [![](https://assets.appsmith.com/git-sync/Buttons.svg) ](https://app.appsmith.com/applications/6289e2937901344ba8d17ca5/pages/6289e2937901344ba8d17ca8) [![](https://assets.appsmith.com/git-sync/Buttons2.svg)](https://app.appsmith.com/applications/6289e2937901344ba8d17ca5/pages/6289e2937901344ba8d17ca8/edit)

------------------------------------------------------------------------------------------------------------------------------------------------------

Why:

- Facebook seller may not have business license/domain to apply the business account
- Difficult to setup the Facebook store
- Rely on opening a Facebook page and upload the photo as well as its captions to sell their product (such as, #1001, White Tshirt, $50)
- Received 200-500 comments per post (#1001 x1, which means buying 1 White Tshirt) and have to contact the buyers one by one to capture the order
- Lost clients and lower the conversion rate

<img width="600" alt="image" src="https://user-images.githubusercontent.com/39978937/204190945-23202d28-6eb9-4b0e-886c-4b7d2121aef5.png">


Use Case:
- Facebook post or live selling
- Use the application to access Facebook API (Graph API)
- Connect the post ID and search the comments automatically
- Setup the target keyword for the search (for example: #1001 x1)
- If any comment matches with the target keyword, send out a checkout message to the commentors via Messenger API
- The checkout message contains (Product name, product price, quantity, comment, and checkout link), shown as below

      Product name: White Tshirt
      Product price: $50
      Quantity: 1
      Comment: #1001x1
      Checkout link: www.checkoutlink-example12345678.com
      
------------------------------------------------------------------------------------------------------------------------------------------------------

All the access:

**Access to the Demo Facebook page, post & comment**
- #Default page ID: 100066535242041
- #Default post ID: 117240846560191
- #Default Page Access Token (probably expired): EAAGQa0V58p4BAPDDYsRQcXt90PNqpfQhpZCj4cvBgvJyq99vZBrHktlTWEsZAxhEHMeZAJzVNTDfJuOS21HuCipnRgd21FHs5VR3kx92oZCwC7XnoB0t53QKZA2kTapYLWBaOonqqAE8iExQutXMMdm2Wca3v9KG1ObZBHcYPOHcATv26SFQE5U 
- #Latest Access Token: EAAGQa0V58p4BAK72bYyEBTYWSZAYqbPHShx91FqicJJKZAixJmhAKRFjHFmj86XcgK61ZCM3rdRHeAS2k23EKtDL2Ghrb9bAVyzMa7p9iMHNGeWhyiRgVzo5yGS3wdfc5NeOle7DYCHoPUzWkQZBJN6QkmqYd2XZCuiyZCMZBXZCQsZBBypmuhIqY


**Send the message to the Demo account**
- #Demo messenger page id = 105667904384152
- #Demo recipient id = 6284271038268550
- Access Token: EAAGQa0V58p4BAK72bYyEBTYWSZAYqbPHShx91FqicJJKZAixJmhAKRFjHFmj86XcgK61ZCM3rdRHeAS2k23EKtDL2Ghrb9bAVyzMa7p9iMHNGeWhyiRgVzo5yGS3wdfc5NeOle7DYCHoPUzWkQZBJN6QkmqYd2XZCuiyZCMZBXZCQsZBBypmuhIqY
- #Facebook account Jason Lau Kai Hoo


Limitation:
#https://developers.facebook.com/docs/messenger-platform/policy/policy-overview#standard_messaging
#Standard message
#Limitation: reply the message within 24 hours - You can send message to any Facebook user if you pass the 24 hours windows

            
------------------------------------------------------------------------------------------------------------------------------------------------------

Connect to the Facebook Post:

![image](https://user-images.githubusercontent.com/39978937/204191813-e19fd828-80d3-46a1-9b50-98eea15d30f5.png)

![image](https://user-images.githubusercontent.com/39978937/204191873-15346512-c35f-46d6-a537-0cadeb64fff7.png)

            export default {
	myVar1: [],
	myVar2: {},
	connectFb: async () => {
		const payload = {
			id: Facebook_ID_connect.text,
			postId: Facebook_Post_Id.text,
			token: Access_token_input.text
		}
		await fetch(`https://graph.facebook.com/${payload.id}_${payload.postId}`, {
			headers: { Authorization: `Bearer ${payload.token}`, 'Content-Type':'application/json' }
		})
			.then((response) => response.json())
			.then((data) => {
				console.log(data)
				storeValue('postData', data, false)
		})
	},
	fetchComments: async () => {
		let facebookPostData;
		
		const payload = {
			id: Facebook_ID_connect.text,
			postId: Facebook_Post_Id.text,
			token: Access_token_input.text
		}
		await fetch(`https://graph.facebook.com/${payload.id}_${payload.postId}?fields=comments{message,id,created_time,from}`, {
			headers: { Authorization: `Bearer ${payload.token}`, 'Content-Type':'application/json' }
		})
			.then((response) => response.json())
			.then((data) => {
			facebookPostData = data.comments;
				storeValue('comments', data.comments, false)
			})
		console.log(facebookPostData);
		return facebookPostData;
	},
	searchComments: () => {
		if (FetchComments.data.comments.data) {
			if (FetchComments.data.comments.data.length > 0) {
				storeValue('targetComments', undefined)
			}
		}

		const targetComments = FetchComments.data.comments.data.filter(item => item.message === search_comment_input.text)
		console.log(targetComments);
		storeValue('targetComments', targetComments, false)
	}
      }
      
------------------------------------------------------------------------------------------------------------------------------------------------------

Search Comment automatically:

	def FB_post_connect_input():
    		print ("To connect your Facebook Page and target post, we need the information")
    
    	#Enter Facebook page id & post id
    		FB_page_id = input("Enter your Facebook Page ID: ")
    		FB_post_id = input("Enter your Facebook Post ID: ")
    
    	#Enter Access Token
    		FB_page_access_token = input("Enter your Facebook Page Access Token: ")
    
    	#Create Get Facebook Post url
    		FB_connect_post_url = "https://graph.facebook.com/"+FB_page_id+"_"+FB_post_id+"?fields=comments{message,id,created_time,from}"
   	 	return FB_page_id,FB_post_id,FB_page_access_token,FB_connect_post_url
		
	FB_page_id, FB_post_id, FB_page_access_token, FB_connect_post_url = FB_post_connect_input()
	
	#Parse the connect input
	def FB_post_connect(FB_connect_post_url,FB_page_access_token):
    	r = requests.get(FB_connect_post_url,
                headers={'Authorization':'Bearer '+ FB_page_access_token,
                         'Content-Type':'application/json'
                        }
                )
    
    	#Check request status
    	print (r)
   	print(r.status_code)
    	if r.status_code == requests.codes.ok:
      	print("OK")
    	
    	return r
	
	#Connect Facebook Post API

	r = FB_post_connect(FB_connect_post_url,FB_page_access_token)
	
	#Configure the message to dataframe
	#Convert into JSON format
	r_json=r.json()
	r_json

	message_data = r_json['comments']['data']
	message_data


![image](https://user-images.githubusercontent.com/39978937/204192093-3c7b5189-88ba-4f0c-91f1-c57b77bf27ea.png)

![image](https://user-images.githubusercontent.com/39978937/204192125-4c038ef4-7635-4de4-9a60-1b69d0e97145.png)


