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
    int data;                              //data in the tree node
    struct TreeNode* left;                //pointer to the left child
    struct TreeNode* right;                     //pointer to the right child
}TreeNode;


//Linked List Node structure

typedef struct ListNode {
    int data;                                //data in the linked listnode
    struct ListNode* next;                  //pointer to the next node
}ListNode;


//Queue Node structure for BFS

typedef struct QueueNode {
    TreeNode* treeNode;                             //pointer to the treenode
    struct QueueNode* next;                          //pointer to the next queue node
} QueueNode;


//Queue structure

typedef struct Queue {
    QueueNode* front;                              //pointer to the front of the queue
    QueueNode* rear;                               //pointer to the rear of the queue
} Queue;


//Function to create a new TreeNode
TreeNode* createTreeNode(int data) {
    TreeNode* newNode = (TreeNode*)malloc(sizeof(TreeNode));                

    newNode->data = data;                         //set the data
    newNode->left =NULL;                         //initialize left child as NULL
    newNode->right = NULL;                  //initialize the right child as NULL
    return newNode;
}


//Function to create a new ListNode

ListNode* createListNode(int data) {
    ListNode* newNode = (ListNode*)malloc(sizeof(ListNode));
    newNode->data = data;             //set the data 
    newNode->next = NULL;            //initialize next pointer as NULL
    return newNode;
}


//Function to create a new QueueNode

QueueNode* createQueueNode(TreeNode* treeNode) {
    QueueNode* newNode =  (QueueNode*)malloc(sizeof(QueueNode));
            newNode->treeNode = treeNode;             //set the tree node
            newNode->next = NULL;                //initialize next pointer as NULL
            return newNode;
}
            

//Function to create an empty queue
Queue* createQueue() {
    Queue* q = (Queue*)malloc(sizeof(Queue));
    q->front = NULL;                 //initialize front as NULL
    q->rear = NULL;              //initialize rear as NULL
    return q;
}


//Function to Enqueue a tree node into the queue
void enqueue(Queue* q, TreeNode* treeNode) {
    QueueNode* newNode =  createQueueNode(treeNode);              //create a new queue node
    if(q->rear == NULL)                           //if the queue is empty
    {
        q->front = q->rear = newNode;               //front and rear point to the new node
        return;
    }
    q->rear->next = newNode;       //add newnode to the end of the queue
    q->rear = newNode;            //update the rear pointer

}


//Function to Dequeue a tree node from the queue
TreeNode* dequeue(Queue* q) {
    if(q->front == NULL)             //if the queue is empty return NULL
        return NULL;
    QueueNode* temp = q->front;           //get the front node
    TreeNode* treeNode = temp->treeNode;            //get the tree node
    q->front = q->front->next;                  //update the front pointer
    if(q->front == NULL)         
    
        q->rear = NULL;           //if the queue is now empty update rear
    
    free(temp);                  //free dequeued node
    return treeNode;          //return treenode            
}


//check if the queue is empty
int isQueueEmpty(Queue* q) {
    return q->front == NULL;             //return 1 if empty, 0 otherwise
}


//Function to convert binary tree to linked list

ListNode* treeToLinkedList(TreeNode* root) {
    if(!root)
        return NULL;           //if tree is empty return NULL

    Queue* q = createQueue();          //create a queue for BFS
    enqueue(q, root);                //Enqueue the root node

    ListNode* head = NULL;        //initialize the linked list head
    ListNode* tail = NULL;        //initialize the linked list tail

    while (!isQueueEmpty(q))    //while the queue is not empty
    {       
        TreeNode* current = dequeue(q);    //dequeue a tree node

        //create a new list node for the linked list
        ListNode* newNode = createListNode(current->data);
        if(!head)       //if this is the first node
        {
            head = newNode;      //set it as the head
        }
        else
        {
            tail->next = newNode;          //link it to the tail
        }
        tail = newNode;                //update the tail pointer

        //Enqueue left and right children of the current tree node
        if(current->left) 
            enqueue(q, current->left);
        
        if(current->right) 
            enqueue(q,current->right);

    }

        return head;        //return the head of the linked list
}

//Function to print linked list
void printLinkedList(ListNode* head)
{
    ListNode* temp = head;             //start with the head node
    while(temp) {                //traverse until the end
        printf("%d", temp->data);             //print the current node's data
        if(temp->next)
        {
            printf(" => ");         //print arrow if not the last node
        }
        temp = temp->next;          //move to the next node
    }
    printf("\n");           //print the newline at the end
}

//Function to create a sample binary tree for demonstration

TreeNode* createSampleTree() {
    TreeNode* root = createTreeNode(1);         //create the root node

    root->left = createTreeNode(2);        //create left child

    root->right = createTreeNode(3);         //create right child

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
    TreeNode* root =  createSampleTree();           //create a sample binary tree
    ListNode* linkedList = treeToLinkedList(root);          //convert to linked list
    printf("Linked List: ");
    printLinkedList(linkedList);            //print the linked list
    return 0;
}


   
