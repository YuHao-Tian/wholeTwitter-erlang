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
首先初始化database, init_database  
然后先打开server端，start()  
其次使用client端，先start()，然后调用catalog()。每次做完一个选项需要重新调用catalog()进行下一步的选择。  
