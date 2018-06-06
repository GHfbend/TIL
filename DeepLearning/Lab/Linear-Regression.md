# Linear Regression

## 그래프 설계

 - H(x) = Wx + b
 '''
 # X and Y data
 x_train = [1, 2, 3]
 y_train = [1, 2, 3]
 
 w = tf.Variable(tf.random_normal([1]), name="weight")
 b = tf.Variable(tf.random_normal([1]), name="bias")
 # Our hypothesis XW + b
 hypothesis = (x_train * W) + b
 '''
 
 - ![Alt text](/images/DL1.PNG)
 '''
 # cost/loss funciton
 cost = tf.reduce_mean(tf.square(hypothesis - y_train))
 '''

 - Gradient Descent Algorithm
 '''
 # Minimize
 optimizer = tf.train(GradientDescentOptimizer(learning_rate=0.01)
 train = optimizer.minimize
 '''

## 그래프 Run/Update 및 결과값 도출

 '''
 # Launch the graph in a session
 sess = tf.Session()
 
 # Initializes global variables in the graph
 sess.run(tf.global_variables_initializer())
 
 # Fit the line
 for step in range(2001):
    sess.run(train)
    if step % 20 == 0:
        print(step, sess.run(cost), sess.run(W), sess.run(b))
 '''