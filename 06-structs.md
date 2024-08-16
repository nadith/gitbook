# 06 - Structs

## Can I get some help on LinkedList?

Yes, here we go. You are encouraged to write the code by yourself and check your code is in line with the code below:

{% tabs %}
{% tab title="main.c" %}
```c
#include <stdio.h>
#include "LinkedList.h"

void printData(void* pData)
{
	printf("APP:PDATA: %d\n", *((int*)pData));
}

void freeData(void* pData)
{
	
}

int main()
{
	LinkedList* pList = createLinkedList();
	int iData1 = 10, iData2 = 20;
	
	insertLast(pList, &iData1);
	insertLast(pList, &iData2);
	
	printLinkedList(pList, &printData);
	freeLinkedList(pList, &freeData);
	
	return 0;
}
```
{% endtab %}

{% tab title="LinkedList.h" %}
```c
#ifndef LINKEDLIST_H
#define LINKEDLIST_H

typedef void (*listFunc)(void* data);

typedef struct LinkedListNode
{
	void* pData;
	struct LinkedListNode* pNext;
} LinkedListNode;

typedef struct LinkedList
{
	LinkedListNode* pHead;
	LinkedListNode* pTail;
	int iSize;	
} LinkedList;

LinkedList* createLinkedList();
void insertLast(LinkedList* list, void* entry);
void* removeLast(LinkedList* list);
void printLinkedList(LinkedList* list, listFunc funcPtr);
void freeLinkedList(LinkedList* list, listFunc funcPtr);

#endif
```
{% endtab %}

{% tab title="LinkedList.c" %}
```c
#include <stdlib.h>
#include <assert.h>
#include "LinkedList.h"

LinkedList* createLinkedList()
{	
	LinkedList* pLinkedList = (LinkedList*) malloc(sizeof(LinkedList));
	pLinkedList->pHead = NULL;
	pLinkedList->pTail = NULL;
	pLinkedList->iSize = 0;

	return pLinkedList;	
}

void insertLast(LinkedList* pList, void* pEntry)
{
	/* Create a new node */
	LinkedListNode* pNode = (LinkedListNode*) malloc (sizeof(LinkedListNode));
	pNode->pData = pEntry;
	pNode->pNext = NULL;
		
	/* If linkedlist is empty */
	if (pList->pHead == NULL)
	{
		assert(pList->pTail == NULL && pList->iSize == 0);		
		pList->pHead = pNode;
	}
	else
	{
		assert(pList->pTail && pList->iSize > 0);	
		pList->pTail->pNext = pNode;
	}
	
	pList->pTail = pNode;
	(pList->iSize)++;
	
}

void* removeLast(LinkedList* pList) 
{ 
	LinkedListNode* pCur = pList->pHead;
	void* pRet = NULL;
	
	while (pCur)
	{		
		if (pCur->pNext == pList->pTail)
		{  /* If linkedlist has more than 1 node */
			
			/* TODO: set pRet = pTail's pData */
			/* TODO: NULLIFY all fields in pTail */
			/* TODO: free pTail */
			/* TODO: set pTail = pCur */
			/* TODO: set pTail->pNext = NULL */
			(pList->iSize)--;
			pCur = NULL;
			
			assert(pList->pTail && pList->pHead && pList->iSize > 0);
			
		}
		else if (/* TODO: If linkedlist has exactly 1 node */)
		{
			/* TODO: set pRet = pTail's pData */
			/* TODO: NULLIFY all fields in pTail */
			/* TODO: free pTail */
			/* TODO: set pTail = pHead = NULL */
			(pList->iSize)--;
			pCur = NULL;
			
			assert(pList->pTail == NULL && pList->pHead == NULL && pList->iSize == 0);
			
		}
		else
		{
			pCur = pCur->pNext;
		}		
	}
	
	return pRet;
}



void printLinkedList(LinkedList* pList, listFunc funcPtr)
{
	LinkedListNode* pCur = pList->pHead;
	
	while (pCur)
	{
		(*funcPtr)(pCur->pData);
		pCur = pCur->pNext;
	}
}

void freeLinkedList(LinkedList* pList, listFunc funcPtr) 
{
	LinkedListNode* pCur = pList->pHead;
	LinkedListNode* pTemp;
	
	while (pCur)
	{
		/* take pCur->pNext to pTemp */
		pTemp = pCur->pNext;
		
		/* clean pCur */
		(*funcPtr)(pCur->pData);
		pCur->pData = NULL;
		pCur->pNext = NULL;
		free(pCur);
		
		/* set pCur = pTemp */
		pCur = pTemp;		
	}	
	
	free(pList);
}

```
{% endtab %}
{% endtabs %}
