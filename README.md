# Binary-Tree-to-linked-list
Convert the below tree structure into a linked list structure, whereas the linked list first element will be the top node in the tree and all the next level nodes are added as next elements in the linked list until all tree nodes are added to the linked list.    

/*
   Name:SHARADA SUTAR
   Date:
   Description:Convert the below tree structure into a linked list structure, whereas the linked list first element will be the top node in the tree and all the next level nodes are added as next elements in the linked list until all tree nodes are added to the linked list.
   Sample Input:
         Tree
            -data
            -left *
            -right *

    
   Sample Output:
               Linked List
                         -data
                         -next *

                         1=> 2=> 3=> 4 =>5 =>6 =>7 =>8 =>9 =>10 =>11 =>12 =>13 =>14 =>15 =>16


*/


#include <stdio.h>
#include <stdlib.h>


//Binary Tree node structure

typedef struct TreeNode {
    int data;
    struct TreeNode* left;
    struct TreeNode* right;
}TreeNode;


//Linked List Node structure

typedef struct ListNode {
    int data;
    struct ListNode* next;
}ListNode;


//Queue Node structure for BFS

typedef struct QueueNode {
    TreeNode* treeNode;
    struct QueueNode* next;
} QueueNode;


//Queue structure

typedef struct Queue {
    QueueNode* front;
    QueueNode* rear;
} Queue;


//Function to create a new TreeNode
TreeNode* createTreeNode(int data) {
    TreeNode* newNode = (TreeNode*)malloc(sizeof(TreeNode));
    newNode->data = data;
    newNode->left =NULL;
    newNode->right = NULL;
    return newNode;
}


//Function to create a new ListNode

ListNode* createListNode(int data) {
    ListNode* newNode = (ListNode*)malloc(sizeof(ListNode));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}


//Function to create a new QueueNode

QueueNode* createQueueNode(TreeNode* treeNode) {
    QueueNode* newNode =  (QueueNode*)malloc(sizeof(QueueNode));
            newNode->treeNode = treeNode;
            newNode->next = NULL;
            return newNode;
}
            

//Function to create an empty queue
Queue* createQueue() {
    Queue* q = (Queue*)malloc(sizeof(Queue));
    q->front = NULL;
    q->rear = NULL;
    return q;
}


//Function to Enqueue a tree node into the queue
void enqueue(Queue* q, TreeNode* treeNode) {
    QueueNode* newNode =  createQueueNode(treeNode);
    if(q->rear == NULL)
    {
        q->front = q->rear = newNode;
        return;
    }
    q->rear->next = newNode;
    q->rear = newNode;

}


//Function to Dequeue a tree node from the queue
TreeNode* dequeue(Queue* q) {
    if(q->front == NULL)
        return NULL;
    QueueNode* temp = q->front;
    TreeNode* treeNode = temp->treeNode;
    q->front = q->front->next;
    if(q->front == NULL)
    
        q->rear = NULL;
    
    free(temp);
    return treeNode;
}


//check if the queue is empty
int isQueueEmpty(Queue* q) {
    return q->front == NULL;
}


//Function to convert binary tree to linked list

ListNode* treeToLinkedList(TreeNode* root) {
    if(!root)
        return NULL;

    Queue* q = createQueue();
    enqueue(q, root);

    ListNode* head = NULL;
    ListNode* tail = NULL;

    while (!isQueueEmpty(q)) {
        TreeNode* current = dequeue(q);

        //create a new list node for the linked list
        ListNode* newNode = createListNode(current->data);
        if(!head) 
        {
            head = newNode;
        }
        else
        {
            tail->next = newNode;
        }
        tail = newNode;

        //Enqueue left and right children of the current tree node
        if(current->left) 
            enqueue(q, current->left);
        
        if(current->right) 
            enqueue(q,current->right);
        

    }


        return head;
}

//Function to print linked list
void printLinkedList(ListNode* head)
{
    ListNode* temp = head;
    while(temp) {
        printf("%d", temp->data);
        if(temp->next)
        {
            printf(" => ");
        }
        temp = temp->next;
    }
    printf("\n");
}

//Function to create a sample binary tree for demonstration

TreeNode* createSampleTree() {
    TreeNode* root = createTreeNode(1);

    root->left = createTreeNode(2);

    root->right = createTreeNode(3);

    root->left->left =  createTreeNode(4);

    root->left->right = createTreeNode(5);

    root->right->left = createTreeNode(6);
    
    root->right->right = createTreeNode(7);

    root->left->left->left =  createTreeNode(8);

    root->left->left->right = createTreeNode(9);

    root->left->right->left = createTreeNode(10);

    root->left->right->right = createTreeNode(11);

    root->right->left->left = createTreeNode(12);

    root->right->left->right = createTreeNode(13);

    root->right->right->left = createTreeNode(14);

    root->right->right->right =  createTreeNode(15);

        root->right->right->right = createTreeNode(16);
    return root;
}

//main function
int main()
{
    TreeNode* root =  createSampleTree();
    ListNode* linkedList = treeToLinkedList(root);
    printf("Linked List: ");
    printLinkedList(linkedList);
    return 0;
}
   
