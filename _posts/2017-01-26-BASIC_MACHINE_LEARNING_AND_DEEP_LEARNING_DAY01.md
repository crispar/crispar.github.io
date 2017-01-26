---
published: true
title: Machine Learning - Inflearn MOOC DAY01
layout: post
author: jungwook
category: Machine Learning
tag:
- machine Learning

---

Reference Page :

[Inflearn 기본적인 머신러닝 딥러닝 강좌](http://bit.ly/2kyZPOV)

[Inflearn 강좌 요약 및 정리](http://pythonkim.tistory.com/8)


## 머신 러닝은 무엇인가?

최근 이세돌과 DeepMind의 Alphago간의 바둑 대결을 통해 머신 러닝에 대한 일반인들의 관심이 높아지고 있다. 일반적으로 바둑의 경우에는 경우의 수가 너무 많고 종합적인 사고 및 판단력이 중요하기 때문에 인간이 유리한 것으로 판단하고 있었으나 이번 대결을 통해 그러한 생각이 잘못되었음을 알게 되었다. 결과적으로 머신러닝은 기계학습을 통해 어떤 주어진 데이터에 대한 최적의 판단 혹은 작업등을 가능하게 하는 것으로 향후 이에 대한 활용 능력이 인간에게 수퍼파워를 가져다 줄 수 있을 것으로 기대하며 이에 대한 활용 능력이 더욱 중요하게 될 것이다.

>향후 업데이트 되는 정보가 있다면, 내용을 좀더 다듬어야 겠다.

## Inflearn 강의는?

+ 머신러닝의 기본적인 개념에 대해 이해하기를 원하는 사람
+ 수학이나 컴퓨터 공학에 대한 배경 지식이 없거나 약한 사람
+ 기본적인 이해를 바탕으로 ML을 사용해 보고 싶은 사람
+ Tensorflow와 python을 사용하기를 원하는 사람

## 강의의 폭표?

+ 머신 러닝에 대한 이해
    + Linear regression, Logistic regression (Classification)
	+ Neural networks, Convolutional Neural Network, Recurrent Nerural Network
+ 머신 러닝을 통한 문제 해결

## 기본 개념

1. 머신 러닝은 무엇인가?

	+ 명시적인 프로그램의 한계
	    + 명시적인 프로그래밍을 통해 기계의 판단을 이끌어내기 위해서는 커버해야하는 룰들이 너무 많은 문제점들이 있다.
	+ 1959 `Arthur Samuel`
	    + 명시적으로 프로그래밍을 하지 말고 컴퓨터에게 학습능력을 주어 먼가를 배우게 하자는 개념이 머신 러닝임

2. 머신 러닝의 학습 방법

	+ Supervised Learning:
		+ `training set`을 통해 학습을 시킴
		+ supervised learning의 형태
			+ Regression : 일정 범위 내로 해당 값을 예측.
			+ Binary classification : 두개의 값중에 하나를 찾는 경우.
			+ Multi-label classification : 특점 범주의 값. 예를들면 점수에 따른 학점.
		+ 예를 들면 고양이를 판단하는 경우 고양이로 레이블 달린 이미지들을 학습시킴

	+ Unsupervised Learning:
		+ Labeling 할 수 없는 데이터가 있다.
		+ Google news grouping
		+ Word clustering
		+ 즉, 데이터를 통해 스스로 학습하게 됨.

3. Tensorflow Basics

	+ Tensorflow는 opensource
	+ python 사용 가능
	+ Data Flow Graph

		![tensorflow](https://camo.githubusercontent.com/4ee55154486232ec9edd8f1a3bad4c4a146f6cfe/68747470733a2f2f7777772e74656e736f72666c6f772e6f72672f696d616765732f74656e736f72735f666c6f77696e672e676966 "BlockChain")
		
		+ Node : 수학적인 연산 (노드 갯수에  제한은 없음.)
		+ Edge : 데이터 ( Data Array 혹은 tensors )

	+ 각각의 연산들은 GPU를 통해 분산 연산이 가능

4. Tensorflow Installation

	기본적으로 나는 pyenv를 사용하므로, pyenv를 통해 anaconda3 4.2.0 버젼을 설치한 상태.

		$ source activate anaconda3-4.2.0
		$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.12.1-cp35-cp35m-linux_x86_64.whl
		$ pip install --ignore-installed --upgrade $TF_BINARY_URL


5. Basic Test Code

		a = tf.constant('Hello, tensorflow!')
		print(a)                                      #Tensor("Const:0", shape=(), dtype=string)

		sess = tf.Session()
		result = sess.run(a)

		# 2.x 버젼에서는 문자열로 출력되지만, 3.x 버젼에서는 byte 자료형
		# 문자여로 변환하기 위해 decode 함수로 변환
		print(result)                                 #b'Hello, tensorflow!'
		print(type(result))                           #<class 'bytes'>
		print(result.decode(encoding='utf-8'))        #Hello, tensorflow!
		print(type(result.decode(encoding='utf-8')))  #<class 'str'>

		#세션 닫기
		sess.close()

6. tensorflow constant

		a = tf.constant(2)
		b = tf.constant(3)

		# with 구문을 벗어날 때, 종료 코드가 있다면 대신 호출해 줌
		# 예외가 발생한 경우에도 보장
		with tf.Session() as sess:
			result = sess.run(a+b)
			print(type(result))             # <class 'numpy.int32'>
			print(result)                   # 5

			# int 자료형과 연산 가능
			print(result + 7)               # 12
			print(type(result + 7))         # <class 'numpy.int64'>

	+ with 구문을 사용하면 리소스를 정리하는 close 함수를 호출하지 않아도 됨. 파일의 열기와 닫기 같은, 쌍을 이루는 코드에 주로 사용. With 구문에서는 변수를 만들 수 없으므로, as를 사용해서 이름을 준다.
	+ 정수를 덧셈한 결과를 저장한 자료형은 numpy 모듈에 있는 int32자료형.
	+ numpy는 C 언어에 있는 배열과 같은 형태로 움직이는 다차원 배열을 기반으로 하는 모듈. 빅데이터, 머신러닝, 과학산술 등의 수치연산이 필요한 모든 경우에 최적의 성능을 보장.
	+ 마지말 줄의 코드에서 7을 더하면 자료형이 int64로 바뀜. 이것은 numpy 고유의 기능. 32비트 숫자 2개를 더하면 오버플로우 발생가능성이 있으므로 수치 연산을 많이 하는 numpy에서는 이러한 오버플로울ㄹ 막기 위해 취해지는 조치임.

7. tensorflow placeHolder

		def placeHolder():
			a = tf.placeholder(tf.int16)
			b = tf.placeholder(tf.int16)

			add = tf.add(a, b)
			mul = tf.mul(a, b)

			with tf.Session() as sess:
				# {a: 2, b: 3}는 딕셔너리
				# key로 'a'와 'b'를 사용하고, value로 2와 3  사용
				# free_dict를 사용하지 않을 경우 None 기본값 적용
				r1 = sess.run(add, feed_dict={a: 2, b: 3})
				r2 = sess.run(mul, feed_dict={a: 2, b: 3})

				print(type(r1))                 # <class 'numpy.int16'>
				print(r1, r2)                   # 5, 6

	+ placeholder는 머신러닝에 전달되는 데이터를 변경하기 위한 수단.
	+ 머신 러닝에게 학습을 시키는 이유는 문제를 풀기 위함이고, 문제를 풀기 위해서는 문제를 알려줘야하는데 그럴 때 사용하는 문법.
