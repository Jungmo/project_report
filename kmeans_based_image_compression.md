# K-means based image compression

Inspired by ['K -Means Clustering-Based Data Compression Scheme for Wireless Imaging Sensor Networks'](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=7312938)

## Argument

~~~~{.python}
import argparse

...

parser = argparse.ArgumentParser()
parser.add_argument('--iter', type = int, default='5', help='the number of itmes of iteration')
parser.add_argument('--clus', type = int, default='4', help='the number of cluster')

args = parser.parse_args()

...

K = args.clus
I = args.iter
~~~~

* 파이썬에서 Argument를 사용할 때에는 argparse 모듈을 사용한다.
* ArgumentParser를 만들고 add_argument로 Argument의 이름, 자료형, 기본값, 도움말을 추가한다.
* parse_args()를 args 변수에 넣는다.
* Argument를 사용할 때에는, args.[Argument의 이름] 으로 호출한다.

## 특정 디렉토리의 리스트를 iteration하기

~~~~{.python}
import os

...

dirpath = "/Users/jungmo/Documents/github/birdnest/repo/train/"

filelist = os.listdir(dirpath)

for filename in filelist:
    print filename
~~~~

* os 모듈을 사용한다.
* Directory path를 파라미터로 사용하여 os.listdir()을 호출한다.
* os.listdir()의 반환값은 해당 Directory path의 list이다.
* list 타입이기 때문에 for 구문으로 간단하게 iteration이 가능하다.

## Random number generator
~~~~{.python}
import random

temp = random.randrange(len(pixel))
~~~~

* random 모듈을 사용한다.
* randrange()의 파라미터에 따라 Random number의 범위가 지정된다.
* Example) randrange(1,7) -> 1~6

```
여기에서 randrange(1,6)이 아니라 randrange(1,7)이라고 썼다는 점에 주의하세요.

"1 이상 7 미만의 난수"라고 생각하시면 이해가 쉽습니다.

출처) WikiDocs, 왕초보를 위한 Python 2.7, 5.3 랜덤모듈
```

## OpenCV를 통해 읽은 이미지의 픽셀읽기.
~~~~{.python}
  image = cv2.imread("image.bmp")
  
  ...
  
        for i in image:
            for j in i:
                ...
~~~~
* 처음에는 image에 대해 하나의 for 구문으로 전체 픽셀에 대해 iteration한다고 생각함.
* 하지만 첫 번째 for 구문은 한 행 전체를 읽어옴
* 이를 다시 for 구문을 통해 해당 행을 iteration 해야함.

## 문제가 됐던 사항
* Centroid가 지정한 K보다 적게 추출됨. (K는 Centroid의 개수)
  * 이는 중복에 의한 문제였는데, centorid list의 length로만 판단을 하여 헛점이 생겼었다.
  * 중복된 값이 있는 상황에서 이걸 바탕으로 dictionary의 key로 사용했기 때문에, key의 개수가 K개가 되지 않았다.
  
