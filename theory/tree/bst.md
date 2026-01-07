# [Binary Search Tree](https://ko.wikipedia.org/wiki/이진_탐색_트리)

루트노드를 기준으로 두 개의 자식노드를 가질 수 있는 노드들로 구성된 트리<br>
노드의 왼쪽 서브트리는 그 노드보다 작은 값들을 담고, 오른쪽 서브트리는 더 큰 값을 가진 값들을 담는다.<br>
어느 노드를 확인 하더라도 하위 트리는 이진 탐색 트리를 만족한다.

추가(삽입), 검색, 제거 등의 기능이 필요하며 정렬된 데이터를 얻을 수 있음<br>
모든 기능이 이진탐색을 통해 빠르게 이루어짐



## 변경 Log
> 노드, 이진탐색 트리의 기본 구조, 함수를 작성함<br>
> 노드를 구조체로 변경하고 트리의 내부에서 선언<br>


## 이진 탐색 트리 코드



```cpp
///////////////////////////////////////////////////////
///BST.h
///////////////////////////////////////////////////////
class BST
{
private :
	struct Node{
		int data;
		Node* left;
		Node* right;

		Node(int value) :data(value), left(nullptr), right(nullptr) {};
	};

private:
	Node* root;
	int count;

public:
	BST()
	{
		root = nullptr;
		count = 0;
	}

	~BST()
	{
		Clear(root);
	}

	int Count() const {
		return count;
	}

	void Add(int value);
	bool Remove(int value);
	bool Search(Node* current, int value);

	void Clear(Node*& ptr);

private:
	void AddNode( Node*& current, int value);
	bool RemoveNode(Node*& current, int value);
	Node* GetMin(Node* current);
};


///////////////////////////////////////////////////////
//BST.cpp
///////////////////////////////////////////////////////
void BST::Add(int value)
{
	if (!root)
		root = new Node(value);

	if (root->data < value)
		AddNode(root->right, value);
	else if (root->data > value)
		AddNode(root->left, value);

	count++;
};

bool BST::Remove(int value)
{
	if (!root)
		return false;
	bool result = RemoveNode(root, value);

	count -= result;
	return result;
};

bool BST::Search(Node* current, int value)
{
	if (!current)
		return false;

	if (current->data == value)
		return true;
	else if (current->data < value)
		return Search(current->left, value);
	else if (current->data > value)
		return Search(current->right, value);
};

void BST::AddNode(Node*& current, int value)
{
	if (!current)
	{
		current = new Node(value);
	}
	else
	{
		if (current->data > value)
			AddNode(current->left, value);
		else
			AddNode(current->right, value);
	}
};

bool BST::RemoveNode(Node*& current, int value)
{
	if (!current)
		return false;
	if (current->data < value)
		return RemoveNode(current->left, value);
	else if (current->data > value)
		return RemoveNode(current->right, value);
	//잎이 2개
	if (current->left && current->right)
	{
		Node* temp = GetMin(current->right);
		current->data = temp->data;
		RemoveNode(current->right, temp->data);
		delete(temp);
		return true;
	}//잎이 1개 == 0개
	else
	{
		Node* temp = current;
		current = nullptr;
		delete(temp);
		return true;
	}
};
BST::Node* BST::GetMin(Node* current)
{
	while (current->left)
		current = current->left;

	return current;
}
void BST::Clear(Node*& ptr)
{
	if (!ptr) return;
	Clear(ptr->right);
	Clear(ptr->left);

	delete(ptr);
};
```