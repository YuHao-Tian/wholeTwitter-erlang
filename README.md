# wholeTwitter-erlang  
# 1. Group members  
Xin Li, Yuhao Tian  
# 2. Project introduction  
1) Database  
The database used in our project is mnesia,it’s only need to be initialized by once. Mnesia is a distributed database so it is very easy to do fault tolerant systems if you have more than one computer. Theoretically speaking, this database can  write data to disk, which enables the operation of dirty data operations and is easy to manage concurrently. This database is maintained by a separate node and called remotely by the server to achieve separation of data and management, and the code can be remotely maintained with slight modifications to the database. This project is to put the database locally on the server, and for optimal performance, no database writing to disk is started.  
2) Methodology  
① Start the server, the server listens to the local port 9191, when the client sends data to the local port 9191, the server processes it and spawn a process to handle the subsequent requests from that client separately. Therefore, the server is a concurrent server, i.e., it can realize the service of multiple clients corresponding to one server, and the server is a blocking server, using passive receiving messages ({active,false}).   
② This project only simulates the socket workflow, simplifies the link process, does not deal with the tcp handshake, and does not verify the html protocol version, the client and the database information transfer does not use the format of http message header (so it does not distinguish get post request), the client front-end does not write web-related programs, can not yet support web display. When the client establishes a connection with the server, binary data is passed, and the data type is json format. After the sender generates the information, it is converted to json format by the xxx method, and the receiver accepts it and then converts it internally to a data type recognizable by erlang. The json format type is as follows: {"cmd":"cmd_instruction","Username":"user",....}   
③ This project takes into account a variety of business logic, including but not limited to the server listening process and the linking process in each stage of the exception capture, user operations to ensure the normal operation of the server under a variety of behaviors (for example, not logged in state to send tweets and subscriptions invalid, login account password verification, can not log out twice, etc.)   
# 3. How to operate  
## 1) Prerequisite  
First of all, we need to open database.erl file, then run code *database:init_database()* as command line.  
Then we need to open server.erl file，running *server:start()* code as command line.  
Now we can start clientt.erl file which takes a role as user. When we want to add a user, we input *client:start()* as command line. We can notice that once a new user was created, there's one more new port notice showed in server.erl's command line.  
Lastly, as server.erl's command line shows 'true', we can call *client:catalog()*.  
## 2) Catalog  
We have 8 operations in our catalog. Separately, they are   
1.log_up(Username,Password)  
2.log_in(Username,Password)  
3.log_off()  
4.subscribe(Username)  
5.tweet()   
6.retweet()
7.search_by_hashtag(Hashtag)   
8.search_by_mention(Mention)   
type index,please    

*Notice: Each time you do an option you need to call catalog() again for the next selection.  
① log_up(Username,Password)  
As we input index 1, the system notices us we need to input Username and Password as follows:  
<img width="199" alt="1671119812090" src="https://user-images.githubusercontent.com/114182493/207907477-fe4a387a-cbcf-4b3d-b75b-181afd1e193d.png">  
Only when result shows ok, it indicates that the registered user is successful.

② log_in(Username,Password)    
Same as log_up function, we need to input registered user's Username and Password as follows:  
<img width="166" alt="1671119975929" src="https://user-images.githubusercontent.com/114182493/207908112-5d2bdaf3-4ad0-47ef-8dd4-4215dbda9c48.png">  
Only when result shows online, it indicates that the user login successfully.   

③ log_off()    
As we want to log off current user, we just need to tap 3. If result shows offline, the user log off successfully.  
<img width="183" alt="1671120174169" src="https://user-images.githubusercontent.com/114182493/207908904-e6ce52ad-a2dd-42fc-9ce3-79cffb642167.png">  

④ subscribe(Username)   
As we want to subscribe some other users, we need to input other users name as follows.  
<img width="168" alt="1671120319478" src="https://user-images.githubusercontent.com/114182493/207909467-71289473-7a6a-4cc9-b8d3-aaf8ff85660c.png">  
Only when result shows ok, it indicates that the user subscribes others successfully.    

⑤ tweet()     
When users want to tweet, they can add any content as text, #topic, @others. Tap 1 means still input and tap 0 means stop adding content.   
<img width="276" alt="1671120599746" src="https://user-images.githubusercontent.com/114182493/207910487-40103b9c-9f15-45df-871d-6f95f5dfceda.png">  
Only when result shows ok and outputs recv, it indicates that the user sends tweet successfully.     

⑥ retweet()  
Retweet means that user can respond tweets which @ yourself. Notic that the index of history array begins at 1. Users can choose any index and respond as tweet format.  
<img width="673" alt="1671120876577" src="https://user-images.githubusercontent.com/114182493/207911773-9928d2b0-2088-4d4a-9c6d-2acc19c1b291.png">    

Only when result shows ok and outputs recv==Sender, it indicates that the user sends retweet successfully.       

⑦ search_by_hashtag(Hashtag)   
When users want to find some hastags, they need to input #hashtag correctly for getting results.  
<img width="297" alt="1671121171574" src="https://user-images.githubusercontent.com/114182493/207913036-d774bc30-4688-4954-bba4-dc0e883cf161.png">    
Only when result shows recv_msg==Sender, users find hashtags successfully and get contents.       

⑧ search_by_mention(Mention)  
Same as search_by_hashtag, users need to input #user correctly for getting results.  
<img width="293" alt="1671121309997" src="https://user-images.githubusercontent.com/114182493/207913564-d44463db-8750-489b-8f41-8e6b1fedc7dd.png">  
Only when result shows recv_msg==Sender, users find mention successfully and get contents. 

## 3) Test  
For testing our Offline Functionality, we need to create one server and three clients. Users need to subscribe to other users or other users @ the user to receive offline messages. After three users have registered and logged in, the third one logs off. The first user sends tweet and @ the log off user(the third one). Now we can notice that the second user which logins can receive the first user's tweet. When offline user(the third one) logins, we can notice that the this user receive the first user's tweet in command line.   
![b3c3cba2d74b501e4be4e492f2e6a91](https://user-images.githubusercontent.com/114182493/207917488-6e5872a9-e14b-4fa2-8a01-61d837abc4ab.png)

