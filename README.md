# wholeTwitter-erlang  
# 1. Group members  
Xin Li, Yuhao Tian  
# 2. Project definition  
Use WebSharper web framework to implement a WebSocket interface to twitter project.  
# 3. Project requirements  
1) Constructing a JSON-based API to represent all messages and their responses (including errors).
2) Use WebSharper to rewrite parts of engine so that the WebSocket interface is implemented.
3) Parts of our client from part 1 need to be rewritten to use WebSockets.  
# 4. How to operate  
首先初始化database, database:init_database().  
然后先打开server端，server:start().  
其次使用client端，先client:start().  
after shows 'true', 调用client:catalog().  
每次做完一个选项需要重新调用catalog()进行下一步的选择。 
We have 8 operations in our catalog. Separately, they are 
1.log_up(Username,Password)  
2.log_in(Username,Password)  
3.log_off()  
4.subscribe(Username)  
5.tweet()   
6.retweet()
7.search_by_hashtag(Hashtag)   
8.search_by_mention(Mention) type index,please    
