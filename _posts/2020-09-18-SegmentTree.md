---
layout: post
title: Segment tree
subtitle: 
gh-repo: 
gh-badge: [star, fork, follow]
tags: 알고리즘
categories : algorithm
comments: true
---

# 알고리즘

## Segment Tree

세그먼트 트리(Segment Tree)는 구간에 대한 정보(구간 합/곱/최대값/최솟값 등)를 빠르게 구해낼 수 있으며 완전 이진트리 형식의 구조를 가지는 자료구조입니다.

이 자료구조의 시간 복잡도는 세그먼트 트리 구성에 O(N log N), 갱신에 O(log N), 구간 값을 알아내는데 O(log N)이 걸리므로 매우 빠르게 구간에 대한 정보를 얻을 수 있습니다. 따라서 일반적인 배열에서 구간정보를 얻는데 O(N)이 걸리므로, 구간정보를 얻어내는데 효율적인 자료구조라고 볼 수 있다.

본 글에서는 배열을 이용한 세그먼트 트리 구현방법에 대해 설명하고자 한다.

부모 노드가 n 번째 노드라면 n*2, n*2+1 번째 노드를 자식노드로 가진다는 특성을 이용하여 1차원 배열에 저장한다. 입력받는 값들은 Leaf Node이기 때문에 뒤쪽 배열에 값을 저장하고,  부모 노드(n/2 노드 or (n-1)/2 노드)에 구간정보를 저장한다. 



![segmentTree1](..\assets\post_img\segmentTree1.jpg)

<center>구간 최솟값을 구하는 세그먼트 트리의 예</center>

구간의 최솟값을 구하는 세그먼트 트리는 먼저 구하고자 하는 배열을 완전 이진트리의 최하단에 위치시킨 후, 모든 부모 노드가 자식 노드들 중 작은 값을 가지게 하며 트리를 완성시킵니다.

이를 통해 트리의 어떤 노드 X는 X의 하위 노드 중 최솟값을 가지게 됨은 자명합니다.

![segmentTree2](..\assets\post_img\segmentTree2.jpg)

예를 들어 아래 배열을 사용한 세그먼트 트리의 구현에서 Tree[1]은 모든 원소의 최솟값이 오게 되고, Tree[2]는 하위 노드인 8~11번지의 최솟값이 들어있게 됩니다.
이들 중 원하는 범위 [L, R]에 필요한 노드를 골라서 구성할 것입니다.

위에서 완성된 세그먼트 트리에서 배열 D에 대한 범위 [L, R]의 최솟값을 찾아 보겠습니다.

![segmentTree3](..\assets\post_img\segmentTree3.jpg)

주어진 L이 0이고 R이 5일 때, 세그먼트 트리로 옮겨보겠습니다.
트리로 옮겨진 점은 소문자 l, r로 표기하고, 이 값을 적당히 옮겨서 구하고자 하는 답인 D[L,R]의 최솟값을 구해 보겠습니다.

![segmentTree4](..\assets\post_img\segmentTree4.jpg)

위의 l과 r사이의 범위를 모두 확인할 수 있는 노드는 다음과 같습니다.

![segmentTree5](..\assets\post_img\segmentTree5.jpg)

빨간 동그라미가 그려진 두 노드 중 최솟값이 우리가 구하려는 범위의 최솟값이 됨을 알 수 있습니다. 두 노드중 왼쪽 노드는 D[0,3]의 최솟값, 오른쪽 노드는 D[4,5]의 최솟값을 가지기 때문입니다.

이 두 노드를 찾는 방법은 l, r을 구하고자 하는 범위를 벗어나지 않게 상위 노드로 옮기는 것입니다.

여기서 l의 의미는 현재 확인해야 할(확인되지 않은) 가장 왼쪽 위치를 포함하는 트리의 노드의 인덱스, r의 의미는 현재 확인해야 할 가장 오른쪽 위치를 포함하는 트리의 노드의 인덱스입니다.

![segmentTree6](..\assets\post_img\segmentTree6.jpg)

<center>배열로 구현된 1-based 이진트리의 구조</center>

배열로 구현된 1-based 세그먼트 트리에 대해 l과 r을 이용하여 답을 구하는 방법은 다음과 같습니다.
세그먼트 트리는 완전 이진트리이기 때문에 위와 같은 구조를 가짐을 참고합니다.

l 이 r보다 작거나 같을 때 다음을 반복합니다.

1) l 이 짝수일 때, l을 2로 나누어 준다.

2) l 이 홀수일 때, Tree[l] 값을 최솟값의 후보로 남겨두고 l을 2로 나누고 1을 더해준다.

3) r 이 홀수일 때 r을 2로 나누어 준다.

4) 4 이 짝수일 때, Tree[r] 값을 최솟값의 후보로 남겨두고 r을 2로 나누고 1을 빼준다.

l이 짝수라면 l의 부모노드의 왼쪽 자식을 의미합니다. l의 상위노드는 l의 범위를 포함하면서 l보다 더 큰 범위를 볼 수 있습니다.

l이 홀수라면 l의 부모노드의 오른쪽 자식을 의미합니다. l의 부모노드의 왼쪽노드는 우리가 확인하려는 범위를 벗어나기 때문에, 현재 트리의 l에 있는 값을 정답의 후보군으로 둔 후, l의 부모노드의 오른쪽 노드로 옮겨 확인되지 않은 오른쪽 범위를 확인해야 합니다.

r이 홀수인 경우는 r이 r의 부모노드의 오른쪽 자식이므로, r의 범위를 포함하여 더 왼쪽 범위를 확인하기 위해 부모노드로 옮겨줍니다.

r이 짝수인 경우는 r의 부모노드는 r의 범위를 벗어난 곳 까지 확인하기 때문에, r의 부모노드는 더이상 의미가 없습니다. 그러므로 기존 트리의 r에 있는 값을 정답의 후보군으로 둔 후, 부모노드의 왼쪽노드를 새로운 r로 두어 더 왼쪽범위까지 확인합니다.

당연히 l과 r의 의미에 따라 l이 r보다 크다면, 위의 루프를 돌 필요가 없습니다.
이를 위의 예제로 확인해보겠습니다.



![segmentTree7](..\assets\post_img\segmentTree7.jpg)

![segmentTree8](..\assets\post_img\segmentTree8.jpg)



![segmentTree9](..\assets\post_img\segmentTree9.jpg)

이 자료구조의 시간 복잡도는 세그먼트 트리 구성에 O(N log N), 갱신에 O(log N), 구간 값을 알아내는데 O(log N)이 걸리므로 매우 빠르게 구간에 대한 정보를 얻을 수 있습니다.

최댓값, 최솟값뿐만 아니라 누적값을 비롯한 여러 정보들을 알맞은 변형을 통하여 빠르게 알아낼 수 있으며, 늦은 전파(lazy propagation)이라는 기법으로 구간 갱신을 효율적으로 개선할 수 있습니다.

그리고 위와 같은 구현은 Bottom-up 구현이라고 합니다. 동작은 유사하지만, 재귀를 활용한 Top-Down 구현 방법도 알아두시면 좋습니다. 시간 복잡도는 같지만, 상수가 다르므로 제한시간을 잘 고려하여 코드를 작성하시면 됩니다.

아래는 Bottom-up 방법으로 구현된 구간의 최솟값을 구할 수 있는 세그먼트 트리 코드입니다.



```
#include <iostream>
#include <algorithm>

using namespace std;


class SegmentTree {
public:
	long* tree;
	int s;

	void display() {
		for (int i = 0; i < s * 2; i++)
			cout << tree[i] <<" ";
	}

	SegmentTree(int n) {
		for (s = 1; s < n; s *= 2) {}

		tree = new long[s * 2];
		for (int i = 1; i < s * 2; i++)
			tree[i] = numeric_limits<long>::max();

	};

	void insert(long* d) {
		int d_size = _msize(d) / sizeof(d);
		for (int i = s; i < s + d_size; i++)
			tree[i] = d[i - s];
		for (int i = s - 1; i >= 1; i--)
			tree[i] = min(tree[i * 2], tree[i * 2 + 1]);//여기 수정하면 최소값, 합, 최대값 트리로 변경 가능
	};


	long getMin(int Left, int Right) {
		int l = Left + s - 1, r = Right + s - 1;
		long rval = numeric_limits<long>::max();
		while (l <= r) {
			if (l % 2 == 0) l /= 2;
			else {
				rval = min(rval, tree[l]);
				l = (l / 2) + 1;
			}
			if (r % 2 == 1) r /= 2;
			else {
				rval = min(rval, tree[r]);
				r = (r / 2) - 1;
			}
		}
		return rval;
	};

	long getSum(int Left, int Right) {
		int l = Left + s - 1, r = Right + s - 1;
		long rval = 0;
		while (l <= r) {
			if (l % 2 == 0) l /= 2;
			else {
				rval += tree[l];
				l = (l / 2) + 1;
			}
			if (r % 2 == 1) r /= 2;
			else {
				rval += tree[r];
				r = (r / 2) - 1;
			}
		}
		return rval;
	};

};
int main() {
	int n,m;
	cin >> n;
	SegmentTree T(n);
	long* v = new long[n];
	for (int i = 0; i < n; i++) {
		cin >> v[i];
	}
	T.insert(v);
	cin >> m;
	while (m--) {
		int a, b;
		cin >> a >> b;
		cout << T.getMin(a, b) << endl;
	}
}
```



#### 링크드리스트를 기반으로 한 코드

```
#include<iostream>
 
using namespace std;
 
class SegmentTree
{
public:
    SegmentTree *pLeft;
    SegmentTree *pRight;
 
    int from, to;
    int value;
 
    SegmentTree()
    {
        pLeft = 0;
        pRight = 0;
        value = -1;
        from = -1;
        to = -1;
    }
 
    void Init(int *arr, int left, int right)
    {
        from = left;
        to = right;
 
        if (left == right)
        {
            value = arr[from];
        }
        else
        {
            int mid = left + (right - left) / 2;
 
            pLeft = new SegmentTree;
            pRight = new SegmentTree;
 
            pLeft->Init(arr, left, mid);
            pRight->Init(arr, mid + 1, right);
 
            value = pLeft->value + pRight->value;
        }
    }
 
    int GetValue(int left, int right)
    {
        if (left <= from && to <= right)
            return value;
        else if (to < left || from > right)
            return 0;
        else
        {
            return pLeft->GetValue(left, right) + pRight->GetValue(left, right);
        }
    }
 
    void Clear()
    {
        if (pLeft != 0)
            if (pLeft->from != pLeft->to)
                pLeft->Clear();
 
        if (pRight != 0)
            if (pRight->from != pRight->to)
                pRight->Clear();
 
        delete pLeft;
        delete pRight;
    }
 
    //구간내의 배열에 특정값을 더할경우
    int PlusValue(int left, int right, int changer)
    {
        if (to < left || from > right)
            return 0;
        else if (from == to)
        {
            value += changer;
            return changer;
        }
        else
        {
            int a = pLeft->PlusValue(left,right,changer);
            int b = pRight->PlusValue(left, right, changer);
            value += a + b;
            return a + b;
        }
    }
 
    //index의 값을 value로 바꾼다
    int SetValue(int index, int number)
    {
        if (index < from || to < index)
        {    
            return value;
        }
        else if(from == index && to == index)
        {
            value = number;
            return value;
        }
        else
        {
            int a = pLeft->SetValue(index, number);
            int b = pRight->SetValue(index, number);
            value = a + b;
            return value;
        }
 
    }
 
 
};
 
 
 
int main(void)
{
    int arr[10] = { 0,1,2,3,4,5,6,7,8,9 };
    SegmentTree trees;
    trees.Init(arr, 0, 9);
 
    
    cout << trees.GetValue(0, 9) << endl;
    cout << trees.GetValue(3, 7) << endl;
 
    trees.PlusValue(3, 7, 2);
 
    for (int i = 0; i < 10; i++)
        cout << trees.GetValue(i, i) << " ";
 
    cout << endl;
 
    cout << trees.GetValue(3, 7) << endl;
    cout << trees.GetValue(0, 9) << endl;
    cout << trees.GetValue(2, 4) << endl;
    
 
    trees.SetValue(4, 30);
 
    for (int i = 0; i < 10; i++)
        cout << trees.GetValue(i, i) << " ";
    
    cout << endl;
    cout << trees.GetValue(3, 7) << endl;
    cout << trees.GetValue(0, 9) << endl;
    cout << trees.GetValue(2, 4) << endl;
 
    trees.Clear();
}
```



참고 자료

https://www.codeground.org/

https://codingfarm.tistory.com/258?category=891914