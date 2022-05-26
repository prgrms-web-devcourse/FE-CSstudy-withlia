# 안정정렬, 불안정정렬

### 정렬 알고리즘

> 정렬 알고리즘은 n개의 숫자가 입력으로 주어졌을 때, 이를 사용자가 지정한 기준에 맞게 정렬하여 출력하는 알고리즘
> 

### 안정정렬

> **중복된 값을 입력 순서와 동일하게 정렬**하는 정렬 알고리즘
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/dd4314d6-9509-4655-bfaf-170222870ed4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T080844Z&X-Amz-Expires=86400&X-Amz-Signature=58c58353fbb4600cea0ac203a32da2d28630926abb7d4ba338b5be921ab16b62&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- **삽입정렬(Insertion Sort)**
- **버블정렬(Bubble Sort)**
- **병합정렬(Merge Sort)**
- 계수정렬

`IMBC`

### 불안정 정렬

> 안정정렬과 반대로 **중복된 값이 입력 순서와 동일하지 않게 정렬**되는 알고리즘
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0b6900ad-d732-444f-9de1-018bd8dee273/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081031Z&X-Amz-Expires=86400&X-Amz-Signature=547f8ca5f74468c9481638ad10cbe2d3436cc39c4d086334c5c1e444b82a0ee7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 퀵정렬
- 선택정렬

`QS`

### 제자리 정렬(**(in-place sorting)**

> 주어진 메모리 공간 외에 **추가적인 공간을 필요로 하지 않는** 정렬 방식
> 
- 주로 swap방식으로 정렬할 경우에 해당

---

# 정렬 알고리즘

## 🧩 삽입 정렬(Insertion Sort)

- 자신보다 작은 데이터가 나올 때까지 계속 비교하면서 앞에서 맞는 위치로 삽입

> **2번째** 원소부터 시작하여 그 **앞(왼쪽)의** 원소들과 **비교하여 삽입할 위치를 지정한 후, 원소를 뒤로 옮기고 지정된 자리에 자료를 삽입**하여 정렬
> 

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        for j in range(i, 0, -1):  # 인덱스 i부터 1까지 1씩 감소하며
            if arr[j] < arr[j-1]:
                arr[j], arr[j-1] = arr[j-1], arr[j]  # swap
            else:  # 자기보다 작은 데이터를 만나면 그 위치에서 멈춤
                break
    return arr
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a0fe896f-5c49-4c61-8a29-20676b69b575/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081057Z&X-Amz-Expires=86400&X-Amz-Signature=2f6a8021b82b73b8637bb77e7d4bdef80f7cd1758298564ad9124aa4edddb274&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

![insertion-sort-001.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3cf492f4-3127-40e2-a99e-6da715872b81/insertion-sort-001.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081125Z&X-Amz-Expires=86400&X-Amz-Signature=bad69777d3a4078d4b5e67de0e0554df7f10fb823bfecaed4088be7f0a76b721&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22insertion-sort-001.gif%22&x-id=GetObject)

### 시간복잡도

- 최악: **O(N^2)**
- 평균: **O(N^2)**
- 최선의 경우: 데이터가 모두 정렬돼 있는 경우 **O(N)**

### 장점

- 대부분의 원소가 ✨**이미 정렬되어 있는 경우, 매우 효율적**
    - 시간복잡도 O(N^2)을 가지는 알고리즘 중에는 가장 빠르다. (연산량이 적으므로)-
- 정렬하고자 하는 배열 안에서 교환하는 방식이므로, **다른 메모리 공간을 필요로 하지 않는다. (in-place sorting)**

### 단점

- **Bubble Sort**와 **Selection Sort**와 마찬가지로, 배열의 길이가 길어질수록 비효율적이다.

---

## 🧩 병합 정렬(Merge Sort)

> **합병 정렬**이라고도 부르며, **분할 정복 방법**을 통해 구현
> 

> 일단 정확히 **반으로 나눠 정렬하고 나중에 합치는 것**
> 

> **합치면서도 정렬**
> 

> **너비가 N, 높이가 logN** → 시간복잡도 **O(NlogN)** 보장
> 

### 분할 정복 (Divide and conquer)

> 큰 문제를 작은 문제 단위로 쪼개면서 해결해나가는 방식
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f49b1c11-c2cd-46a6-b4ee-4fce99ebed68/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081206Z&X-Amz-Expires=86400&X-Amz-Signature=28e55d2f0ade4acb826d621c12c949585b80f8c67e3872f30147f96a7d460239&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- 반으로 나누는 과정: **logN**
- **정렬이** 되어있는 걸 **합치는** 과정에서 **비교** : **N**
- 즉, 위 예시에서 가로 8 세로 3  → **NlogN**

```python
**# 재귀적으로 구현**
def merge_sort(arr):
    if len(arr) < 2:
        return arr

    mid = len(arr) // 2

		# 분할
    low_arr = merge_sort(arr[:mid])
    high_arr = merge_sort(arr[mid:])

    merged_arr = []
    l = h = 0
    while l < len(low_arr) and h < len(high_arr):
        if low_arr[l] < high_arr[h]:
            merged_arr.append(low_arr[l])
            l += 1
        else:
            merged_arr.append(high_arr[h])
            h += 1
    merged_arr += low_arr[l:]
    merged_arr += high_arr[h:]
    return merged_arr
```

### 시간복잡도

- **최선, 평균, 최악** 모두 **O(NlogN)**

### 장점

- 퀵정렬과 함께 굉장히 빠르다.
- 데이터의 분포에 영향을 덜 받아 정렬되는 시간은 항상 동일

### 단점

- 구현 과정에서 **임시 배열**이 필요해 **메모리 공간을 더 필요로 한다.**

---

## 🧩 버블 정렬(Bubble Sort)

> **서로 인접한 두 원소의 대소를 비교**하고**, 자리를 교환하며 정렬하는 알고리즘**
> 

![bubble-sort-001.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e9568081-7f20-4fb1-bcb4-1207b304d2cb/bubble-sort-001.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081231Z&X-Amz-Expires=86400&X-Amz-Signature=b9cc284f22f4531cf54310b41357ed8d8309d72baf2f0f95f187324a6ce6402b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22bubble-sort-001.gif%22&x-id=GetObject)

```python
def bubble_sort(arr):
    n = len(arr)  # 배열의 크기를 측정

    # 배열의 크기만큼 반복
    for i in range(n):
      # 배열의 총 크기에서 i의 값과 1을 뺀 만큼 반복
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]  # swap
    return arr
```

### 시간복잡도

- **최선, 평균, 최악** 모두 **O(N^2)**
- flag 처리를 통해 버블 정렬을 **최적화** 시 **최선**일 경우 **O(N)**

```python
def bubble_sort(arr):
    n = len(arr)
    swapped = False
    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]  # swap
                swapped = True
        if not swapped:
            break
    return arr
```

### 장점

- 정렬하고자 하는 배열 안에서 교환하는 방식이므로, **다른 메모리 공간을 필요로 하지 않는다. (in-place sorting)**

### 단점

- 시간복잡도가 최악, 최선, 평균 모두 **O(n^2)**으로, **굉장히 비효율적**

---

## 계수 정렬(Counting Sort)

> 각각의 데이터가 **몇번씩 등장했는지를 세는** 방식으로 동작
> 
- 특정한 조건이 부합할 때만 사용할 수 있지만 매우 빠른 정렬 알고리즘
    - 특정 조건 1: **데이터값이 양수**
    - 특정 조건 2: **값의** **범위**가 **너무 크지 않아야 한다**. (메모리 사이즈를 넘어서는 안된다.)
- **데이터의 개수가 N**, 데이터(양수) 중 **최댓값이 K**일 때, 최악의 경우에도 수행시간 **O(N+K)** 보장

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3d86eb82-d836-419b-a349-4ab79a31ffcd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081312Z&X-Amz-Expires=86400&X-Amz-Signature=29e5787efd2e7117aa8e085e85f7e8dafc9f297f2da94a5d943f880b29ded273&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```python
def counting_sort(array):
    # 모든 범위를 포함하는 리스트 선언(모든 값은 0으로 초기화)
    n = len(array)
    k = max(array)

    count = [0] * (k + 1) # 개수를 셀 배열
    result = [0] * n # 결과를 저장할 배열

    for i in range(n):
        count[array[i]] += 1  # 각 데이터에 해당하는 인덱스의 값 증가

    for i in range(k):  # 누적합 구하기
        count[i+1] += count[i]

    for i in range(n): # 누적합이 가리키는 인덱스를 바탕으로 결과에 숫자를 집어넣기
        result[count[array[i]] - 1] = array[i]
        count[array[i]] -= 1

    return result
```

### 시간복잡도

- **최선, 평균, 최악** 모두 **O(N+K)**

### 장점

- **동일한 값**을 가지는 데이터가 **여러 개** 등장할 때 효과적이다.

### 단점

- 배열에 큰 수가 들어갈 수록 메모리를 많이 잡아먹기 때문에 좋지 않다.

---

## 퀵 정렬(Quick Sort)

> 퀵 정렬 또한 **분할 정복(divide and conquer) 방법**을 통해 주어진 배열을 정렬한다.
> 
- Merge Sort와 달리 **Quick Sort**는 배열을 **비균등하게 분할**한다.

> 배열 가운데서 하나의 원소를 고른다. 이걸 **피벗(pivot)** 이라고 한다.
> 

> 피벗 **앞에는** 피벗보다 **값이 작은 모든 원소들이 오고**, 피벗 **뒤에는** 피벗보다 **값이 큰 모든 원소들이 오도록** 피벗을 기준으로 **배열을 둘로 나눈다.**
> 

> **분할된 두 개의 작은 배열**에 대해 **재귀**적으로 이 과정을 **반복**
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d5a388ac-8d4f-4d40-a753-cca6d54f9306/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081326Z&X-Amz-Expires=86400&X-Amz-Signature=781650bb4f574c7e31cb78d9d333a87384820012d6f071a966374b943d4e9631&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    lesser_arr, equal_arr, greater_arr = [], [], []
    for num in arr:
        if num < pivot:
            lesser_arr.append(num)
        elif num > pivot:
            greater_arr.append(num)
        else:
            equal_arr.append(num)
    return quick_sort(lesser_arr) + equal_arr + quick_sort(greater_arr)
```

### 시간복잡도

- 최악: **O(N^2)**
- 평균: **O(NlogN)**
- 최선: **O(NlogN)**

### 최적화

- 정렬하고자 하는 배열 안에서 교환하는 방식이므로, **다른 메모리 공간을 필요로 하지 않는다. ((in-place sorting)**
    
    ![quick-sort-001.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2494762a-a0d9-4b38-ad5d-ba3cac936630/quick-sort-001.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081345Z&X-Amz-Expires=86400&X-Amz-Signature=b7909ef32617f80ae12675b43bcde0f86b336970cdab8e4f07b85b9a3990fe3b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22quick-sort-001.gif%22&x-id=GetObject)
    
    ```python
    # swap 방식 최적화 
    array = [5, 7, 9, 0, 3, 1, 6, 2, 4, 8]
    
    def quick_sort(array, start, end):
    	if start >= end: # 원소가 1개인 경우 종료
    		return
    	pivot = start # pivot은 첫 번째 원소
    	left = start + 1
    	right = end
    	while left <= right :
    		# 피벗보다 큰 데이터를 찾을 때까지 반복
    		while(left <= end and array[left] <= array[pivot]):
    			left +=1
    		# 피벗보다 작은 데이터를 찾을 때까지 반복
    		while(right > start and array[right] >= array[pivot]):
    			right -= 1
    		if(left > right): **# 엇갈렸다면 작은 데이터와 피벗을 교체**
    			array[right], array[pivot] = array[pivot], array[right]
    		else:
    			array[left], array[right] = array[right], array[left]
    	
    	# 분할 이후 왼쪽 부분과 오른쪽 분에서 각각 정렬 수행
    	quick_sort(array, start, right -1)
    	quick_sort(array, right+1, end)
    
    quick_sort(array, 0, len(array) -1)		
    print(array)
    ```
    

### 장점

- 병합정렬과 함께 굉장히 빠르다.
    - C언어 정렬 라이브러리도 퀵정렬 기반으로 작성 되어있다.

### 단점

- **거의 대부분 정렬된 배열에 대해서는** Quick Sort의 불균형 분할에 의해 **오히려 수행시간이 더 많이 걸린다.**

---

## 선택 정렬(Selection Sort)

- 자리를 이동시키며 그 자리에 넣을 최솟값을 선택해 찾아 넣는다.

> 해당 순서에 원소를 **넣을 위치는 이미 정해져 있고,** 어떤 원소를 넣을지 **선택**하는 알고리즘
> 

> 주어진 배열 중에 **최솟값**을 찾는다.
> 

> 그 값을 **맨 앞에 위치한 값과** **교체**한다.
> 

> 맨 처음 위치를 **뺀 나머지 배열을 같은 방법으로 교체**한다.
> 

![selection-sort-001.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ff7be3af-919f-4e72-ac06-a3c1d42f38d8/selection-sort-001.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220511%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220511T081408Z&X-Amz-Expires=86400&X-Amz-Signature=bcb809c8768122b52fa4895ee044824e4f56f2643cc3cf7d19db5e6c0890a240&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22selection-sort-001.gif%22&x-id=GetObject)

```python
def selection_sort(array):
    for i in range(len(array)):
        min_index = i  **# 처음에는 가장 앞쪽 인덱스로**
        for j in range(i+1, len(array)):
            if array[min_index] > array[j]:
                min_index = j  **# 가장 작은 원소의 인덱스**
        array[i], array[min_index] = array[min_index], array[i]  **# swap**
    return array
```

### 시간복잡도

- 최선, 평균, 최악의 경우 시간복잡도는 모두 **O(n^2)**로 동일

### 장점

- Bubble Sort와 마찬가지로 **정렬하고자 하는 배열 안에서 교환**하는 방식이므로, **다른 메모리 공간을 필요로 하지 않는다. (in-place sorting)**

### 단점

- 시간복잡도가 O(n^2)으로, **비효율적**
- **불안정 정렬(Unstable Sort)**이다.

---

### 파이썬 내부 정렬 알고리즘?

- 파이썬 표준 라이브러리에서 기본으로 제공하는 정렬 라이브러리는 **최악의 경우 O(NlogN)**을 보장한다.
- **Tim sort**
    - 2002년 소프트웨어 엔지니어 Tim Peters에 의하여 Tim sort가 등장했다. 이 정렬 알고리즘은 **Insertion sort와 Merge sort를 결합하여 만든 정렬**
    - Merge sort를 기반으로 하되, 좀 더 효율적으로 Insertion sort를 일부 사용

---

### JS에서 내부 정렬 알고리즘?

- ECMAscript에서 따로 정렬 알고리즘에 대한 표준을 제시하지 않았다.
- Mozilla는 `MergeSort`를 사용하지만 오늘날 **Chrome의 v8 엔진**에서는 더 작은 배열에 `QuickSort`및 `InsertionSort`를 사용

---

### 출처

[동빈나 실전알고리즘 강좌](https://www.youtube.com/watch?v=qQ5iLNjpxSk&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz)

[https://gyoogle.dev/blog/](https://gyoogle.dev/blog/)

[https://www.daleseo.com/](https://www.daleseo.com/)

[https://www.zerocho.com/category/Algorithm/post/58006da88475ed00152d6c4b](https://www.zerocho.com/category/Algorithm/post/58006da88475ed00152d6c4b)

[Tim sort에 대해 알아보자](https://d2.naver.com/helloworld/0315536)
