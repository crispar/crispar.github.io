---
published: true
title: Machine Learning - Inflearn MOOC DAY02
layout: post
author: jungwook
category: Machine Learning
tag:
- machine Learning

---

Reference Page :

[Inflearn 기본적인 머신러닝 딥러닝 강좌](http://bit.ly/2kyZPOV)

[Inflearn 강좌 요약 및 정리](http://pythonkim.tistory.com/7)


## 1. 회기 분석

점들이 퍼져있는 형태에서 패턴을 찾아내고, 이 패턴을 활용해서 무언가를 예측하는 분석.

## 2. Lenear Regression

2차원 좌표에 분포된 데이터를 1차원 직선 방정식을 통해 표현되지 않는 데이터를 예측하기 위한 분석 모델.

## 3. Hypothesis

Linear Regression에서 사용하는 1차원 방정식을 가리키는 용어로, 우리말로 가설이라고 함. 수식에서 h(X) 혹은 H(X)로 표현함.

H(x) = Wx + b 에서 Wx + b는 x에 대한 1차 방정식으로 직선을 표현. 기울기에 해당하는 W(Weight)와 절편에 해당하는 b(bias)가 반복되는 과정에서 계속 변경되고, 마지막 루프에서 바뀐 최종 값을 사용해서 데이터 예측을 하게됨. 최종적으로 나온 가설을 모델(model)이라고 부르고, "학습되었다"라고 함.

## 4. Cost(비용)

Hypothesis 방정식에 대한 비용(cost)으로 방정식의 결과가 크게 나오면 좋지 않다고 이야기하고, 루프를 돌 때마다 W와 b를 비용이 적게 발생하는 방향으로 수정.

Linear Hypothesis          |  Which Hypothesis is better
:-------------------------:|:-------------------------:
![]({{ site.baseurl }}/images/lect_02_01.jpg)  |  ![]({{ site.baseurl }}/images/lect_02_02.jpg)

왼쪽의 경우 데이터가 (1,1) (2,2) (3,3)인 경우로 최적의 Hypothesis 파랑색 선이 됨. 데이터 양이 많아질수록 모든 케이스를 만족하는 것은 어려우므로 Cost가 최소가 되는 선을 찾는 것이 학습의 목적

Cost function          |  Cost function 
:-------------------------:|:-------------------------:
![]({{ site.baseurl }}/images/lect_02_03.jpg)  |  ![]({{ site.baseurl }}/images/lect_02_04.jpg)

+ 결과적으로 각 값들의 분산 평균이 최소가 되는 것이 Cost가 최소가 되는 것임.
+ 제곱을 하는 이유는 뺄샘을 하게 되면 짓선 위치에 따라 음수와 양수가 섞어나와 정상적인 차이를 구할 수 없음.
+ 절대값도 있으나, 제곱이 가장 간단한 어떠한 케이스에도 양수가 됨.

## 5. Linear Regression

	import tensorflow as tf

	x_data = [1, 2, 3]
	y_data = [1, 2, 3]

	# try to find values for w and b that compute y_data = W * x_data + b
	W = tf.Variable(tf.random_uniform([1], -1.0, 1.0))
	b = tf.Variable(tf.random_uniform([1], -1.0, 1.0))

	# my hypothesis
	hypothesis = W*x_data + b

	# Simplified cost function
	cost = tf.reduce_mean(tf.square(hypothesis - y_data))

	# minimize
	rate = tf.Variable(0.1)  # learning rate, alpha
	optimizer = tf.train.GradientDescentOptimizer(rate)
	train = optimizer.minimize(cost)

	# before starting, initialize the variables. We will 'run' this first
	init = tf.initialize_all_variables()

	# launch the graph
	sess = tf.Session()
	sess.run(init)

	# fit the line
	for step in range(2001):
		sess.run(train)
		if step %20 == 0:
			print('{:4} {} {} {}'.format(step, sess.run(cost), sess.run(W), sess.run(b)))

	# learns best fit is W: [1] b:[0]

## 6. placeholder

	import tensorflow as tf

	x_data = [1, 2, 3, 4]
	y_data = [2, 4, 6, 8]

	# try to find values for w and b that compute y_data = W * x_data + b
	W = tf.Variable(tf.random_uniform([1], -100, 100))
	b = tf.Variable(tf.random_uniform([1], -100, 100))

	X = tf.placeholder(tf.float32)
	Y = tf.placeholder(tf.float32)

	# my hypothesis
	hypothesis = W*X + b

	# Simplified cost function
	cost = tf.reduce_mean(tf.square(hypothesis - Y))

	# minimize
	rate = tf.Variable(0.1)  # learning rate, alpha
	optimizer = tf.train.GradientDescentOptimizer(rate)
	train = optimizer.minimize(cost)

	# before starting, initialize the variables. We will 'run' this first
	init = tf.initialize_all_variables()

	# launch the graph
	sess = tf.Session()
	sess.run(init)

	# fit the line
	for step in range(2001):
		sess.run(train, feed_dict={X:x_data, Y:y_data})
		if step %20 == 0:
			print(step, sess.run(cost, feed_dict={X:x_data, Y:y_data}), sess.run(W), sess.run(b))

	print(sess.run(hypothesis, feed_dict={X:5}))
	print(sess.run(hypothesis, feed_dict={X:2.5}))
	print(sess.run(hypothesis, feed_dict={X: [2.5, 5]}))

	# learns best fit is W: [1] b:[0]

## 7. Linear Regression의 cost 최소화 알고리즘의 원리

Hypothesis and Cost        |  Simplified hypothesis
:-------------------------:|:-------------------------:
![]({{ site.baseurl }}/images/lec_03_01.jpg)  |  ![]({{ site.baseurl }}/images/lec_03_02.jpg)

왼쪽은 hypothesis와 cost 함수에 대한 공식이고, 오른쪽은 b(bsis)를 없앤 공식. b를 없애고, cost 함수에 H(x) 대신 Wx를 직접 입력.

What cost(W) looks like?   |  How to minimize cost?
:-------------------------:|:-------------------------:
![]({{ site.baseurl }}/images/lec_03_03.jpg)  |  ![]({{ site.baseurl }}/images/lec_03_04.jpg)

왼쪽은 단순 버젼의 공식으로 cost를 비용한 내용이고 오른쪽은 cost와 W값을 이용해 그래프를 그린 것.

## 8. Gradient descent algorithm

+ 경사를 따라 내려가는 알고리즘
+ Minimize cost function에서 사용됨.
+ 다양한 최저 계산에 사용
+ Cost 함수에 대한 최소의 기울기(W)와 y 절편(b) 탐색
+ 변수가 여러 개인 경우에도 적용이 가능함.

## 9. Gradient descent algorithm 동작 원리

+ 처음에는 추측값으로 시작 (0 일수도 있고 다른 값일 수도 있음)
+ W와 b를 아주 조금씩 변경해가면서 비용을 최소화함.
+ 인자가 변경될 때마다, cost가 줄어드는 방향의 기울기를 선택
+ 이를 반복함.
+ 최소 값을 갖을 때까지 계속 함.
	+ 어디서 시작하든 결과적으로 최소값을 가져야함.

Formal Definition  |  Formal Definition    |  Formal Definition
:-------------------------:|:-------------------------:|:-------------------------:
![]({{ site.baseurl }}/images/lec_03_07.jpg)  |  ![]({{ site.baseurl }}/images/lec_03_08.jpg)    |  ![]({{ site.baseurl }}/images/lec_03_09.jpg)

Cost계산시에 기본적으로 아이템의 개수로 나누어 평균을 구하는데, 이 값을 평균 오차상 m과 2m의 경우 크게 차이가 없으므로 해당 값을 2m으로 한다. ( 나중에 미분 공식을 이용할 때 해당 값을 제거해버리기 위해서 그런 것으로 보임.)

$$
W := W - \alpha\dfrac{\partial}{\partial{W}}cost(W)
$$

위의 식의 경우 W = W - 변화량을 의미하며, 이 때 변화량의 값은 cost(W)를 W에 대해서 미분하여 기울기 변화를 찾아내는 방법이다. 결론적으로 cost function의 경우

$$
W := W - \alpha\dfrac{\partial}{\partial{W}}\dfrac{1}{2m}\sum_{i=1}^m(Wx^{(i)} - y^{(i)})^2
$$

공식을 풀어서 쓰면 결과적으로 위와 같이 되는데, cost부분을 미분하게 되면,

$$
W := W - \alpha\dfrac{1}{2m}\sum_{i=1}^m2(Wx^{(i)} - y^{(i)})x^{(i)}
$$

2로 나누게 되면 아래와 같은 공식으로 나오게 된다.

$$
W := W - \alpha\dfrac{1}{m}\sum_{i=1}^m(Wx^{(i)} - y^{(i)})x^{(i)}
$$

$\alpha$ 는 우리가 임의로 추가하는 상수로 미분에는 참가하지 않으나 $\alpha$가 너무 크면 오버슈팅(overshooting)이 될수 있고, 너무 작으면 최소 비용에 수렴할 때까지 너무 오래 걸리게 된다. 이 값은 여러번에 걸쳐 테스트하면서 결정해야 하는 중요한 값이지만, 역시 미분에는 참여하지 않는다.

> 그나저나 미분 / 적분도 좀 공부해야 겠군;; 하나도 기억이 안나..

## 10. Minimizing Cost

하기 코드의 목정은 최소 비용을 찾는 것이 아니라 최소 비용을 포함하고 있는 W의 값까지 포함해서 일정 범위의 W에 대한 cost를 계산해서 Cost가 어떻게 변화하는지 보여주는 것이 목적
   
	import tensorflow as tf

	X = [1., 2., 3.]
	Y = [1., 2., 3.]

	# 데이터의 개수
	m = len(X)

	# 기울기(W)를 바꾸면서 비용을 계산해서 보여주려는 것이 목적이므로 W를 placeholder로 처리
	# 여기서는 y 절편에 해당하는 b는 생략하고 있음.
	W = tf.placeholder(tf.float32)

	hypothesis = tf.mul(W, X)
	cost = tf.reduce_sum(tf.pow(hypothesis-Y,2))/m

	init = tf.initialize_all_variables()

	sess = tf.Session()
	sess.run(init)

	# 그래프로 표시하기 위해 데이터를 누적할 리스트
	W_val, cost_val = [], []

	# 0.1 단위로 증가할 수 없어서 -30부터 시작. 그래프에는 -3에서 5까지 표시됨.
	for i in range(-30,51):
		xPos = i*0.1
		yPos = sess.run(cost, feed_dict={W:xPos})

		print('{:3.1f}, {:3.1f}'.format(xPos,yPos))

		# 그래프에 표시할 데이터 누적. 단순히 리스트에 갯수를 늘려나감
		W_val.append(xPos)
		cost_val.append(yPos)

	sess.close()

## 11. placeholder and custom gradient descent algorithm

	import tensorflow as tf

	x_data = [1., 2., 3., 4.]
	y_data = [1., 3., 5., 7.]           # x와 y의 관계가 모호하다. cost가 내려가지 않는 것이 맞을 수도 있다.

	# 동영상에 나온 데이터셋. 40번째 위치에서 best fit을 찾는다. 이번에는 사용하지 않음.
	# x_data = [1., 2., 3.]
	# y_data = [1., 2., 3.]

	W = tf.Variable(tf.random_uniform([1], -10000., 10000.))        # tensor 객체 반환

	X = tf.placeholder(tf.float32)      # 반복문에서 x_data, y_data로 치환됨
	Y = tf.placeholder(tf.float32)

	hypothesis = W * X
	cost = tf.reduce_mean(tf.square(hypothesis - Y))

	# 동영상에서 미분을 적용해서 구한 새로운 공식. cost를 계산하는 공식
	mean    = tf.reduce_mean(tf.mul(tf.mul(W, X) - Y, X))   # 변경된 W가 mean에도 영향을 준다
	descent = W - tf.mul(0.01, mean)
	# W 업데이트. tf.assign(W, descent). 호출할 때마다 변경된 W의 값이 반영되기 때문에 업데이트된다.
	update  = W.assign(descent)

	init = tf.initialize_all_variables()

	sess = tf.Session()
	sess.run(init)

	for step in range(50):
		uResult = sess.run(update, feed_dict={X: x_data, Y: y_data})    # 이 코드를 호출하지 않으면 W가 바뀌지 않는다.
		cResult = sess.run(cost, feed_dict={X: x_data, Y: y_data})      # update에서 바꾼 W가 영향을 주기 때문에 같은 값이 나온다.
		wResult = sess.run(W)
		mResult = sess.run(mean, feed_dict={X: x_data, Y: y_data})

		# 결과가 오른쪽과 왼쪽 경사를 번갈아 이동하면서 내려온다. 기존에 한 쪽 경계만 타고 내려오는 것과 차이가 있다.
		# 최종적으로 오른쪽과 왼쪽 경사의 중앙에서 최소 비용을 얻게 된다. (생성된 난수값에 따라 한쪽 경사만 타기도 한다.)
		# descent 계산에서 0.1 대신 0.01을 사용하면 오른쪽 경사만 타고 내려오는 것을 확인할 수 있다. 결국 step이 너무 커서 발생한 현상
		print('{} {} {} [{}, {}]'.format(step, mResult, cResult, wResult, uResult))

	print('-'*50)
	print('[] 안에 들어간 2개의 결과가 동일하다. 즉, update와 cost 계산값이 동일하다.')

	print(sess.run(hypothesis, feed_dict={X: 5.0}))
	print(sess.run(hypothesis, feed_dict={X: 2.5}))

	sess.close()
