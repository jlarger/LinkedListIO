/*# LinkedListIO
A program that reads 6 e-mail addresses and passwords from a file, stores in a Linked List, and then does standard procedures (traverse, remove, and find) on the list. It then writes the data back to the same file.
*/
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <ctype.h>


/*
 *	James Larger
 *
 *	4/26/17
 *
 *
 */

typedef struct
{
   char username[50];
   char password[20];
}INFORMATION;

typedef struct
{
	unsigned int block;
	INFORMATION * dataPTR;
	struct node * next;
}NodeType;



void startList(NodeType ** headPTR, INFORMATION * newInfoPTR);	
void traverse(NodeType * head);
void removeNode(NodeType * head, char userChoice[]);
void findData(NodeType * head, char userChoice[]);
void writeToFileAfterExiting(INFORMATION * newInfoPTR1, INFORMATION * newInfoPTR2, INFORMATION * newInfoPTR3, INFORMATION * newInfoPTR4, INFORMATION * newInfoPTR5, INFORMATION * newInfoPTR6);
void writeToFile(NodeType * head);





int main(int argc, char ** argv)
{
	
	INFORMATION info1;
	INFORMATION info2;
	INFORMATION info3;
	INFORMATION info4;
	INFORMATION info5;
	INFORMATION info6;

	FILE * fileOfInformation;

	fileOfInformation = fopen("exam3.txt", "r");

	fscanf(fileOfInformation, "%s %s %s %s %s %s %s %s %s %s %s %s", info1.username, info1.password, info2.username, info2.password, info3.username, info3.password, info4.username, info4.password, info5.username, info5.password, info6.username, info6.password);

	
	NodeType * head = 0;

	startList(&head, &info1);

	startList(&head, &info2);

	startList(&head, &info3);

	startList(&head, &info4);

	startList(&head, &info5);

	startList(&head, &info6);

	char userChoice;
	char removalChoice[10];
	char editChoice[10];

	while(userChoice != '4')
	{

		
		printf("Hello! I have retrieved your list from the file. What would you like to do?\n");
		printf("1. Print the list. \n");
		printf("2. Remove an entry.\n");
		printf("3. Find an entry.\n");
		printf("4. Exit the program.\n");


		scanf("%c", &userChoice);

	
	
		
		switch(userChoice)
		{
				
			case '1':
			traverse(head);
			break;
			
			case '2':
			printf("Will remove an entry.\n");
			printf("Which node would you like to be removed?\n");
			scanf("%s", &removalChoice);
			removeNode(head,removalChoice);
			traverse(head);
			break;
			
			case '3':
			printf("Which one would you like to find?\n");
			scanf("%s", &editChoice);
			findData(head, editChoice);
			traverse(head);
			break;
			
			case '4':
			writeToFileAfterExiting(&info1, &info2, &info3, &info4, &info5, &info6);
			//writeToFile(head);
			exit(0);
			break;
			/*default:
			printf("Wrong key!\n");*/
			
	
	}

	
	}	


	fclose(fileOfInformation);






}


/*
 * Starts the list by passing the head of the list and struct INFORMATION
 */

void startList(NodeType ** headPTR, INFORMATION * newInfoPTR)
{
	NodeType * newNodePTR;
	NodeType * ptr = *headPTR;
	NodeType * prevPTR = 0;
	
	if(newInfoPTR == 0) return;

	newNodePTR = (NodeType *) malloc(sizeof(NodeType));

	newNodePTR -> dataPTR = newInfoPTR;

	newNodePTR -> next = 0;
	
		
  	if(ptr == 0) 
 	{
    		*headPTR = newNodePTR;
    		return;
  	}
	else
	{
		while(ptr -> next != 0)
		{
			ptr = ptr -> next;
		}
	}
  	
	ptr -> next = newNodePTR;
  	


}

/*
 * Printing the list
 */
void traverse(NodeType * head)
{
	INFORMATION * dataPtr;

	/*
	 * could do File I/O with this?
	 *
	 */

	while(head != 0)
	{
		dataPtr = head -> dataPTR;    
		printf("Username: %s\n", (dataPtr -> username));
		printf("Password: %s\n", (dataPtr -> password));
		head = head -> next;
	}
}


/*
 *	Function is removing the next node instead of the one found...
 *	Need to figure out why
 */

void removeNode(NodeType * head, char userChoice[])
{
	int x;
	NodeType * nextNode = NULL;
	NodeType * prevNode = NULL;
	NodeType * temp = head;
	
	INFORMATION * dataPtr;

	while(head != 0)
	{


		dataPtr = head -> dataPTR;
		head = head -> next;
		
		if(strcmp(dataPtr->username, userChoice) == 0)
		{

			printf("Node to be removed found!!\n");
			
			nextNode = (*head).next;
			
			*head = *nextNode;
			
			free(nextNode);
					

		}

		/*
		 * similar to the findData function in terms of error handling
		 *
		 */

		else if(strcmp(dataPtr->username, userChoice) > 0)
		{
			printf("Username too short.\n");
		}

		else if(strcmp(dataPtr->username, userChoice) < 0)
		{
			printf("Username too long.\n");
		}
		
	}	
}

/*
 * Finds a specific username in the list
 */

void findData(NodeType * head, char userChoice[])
{
	INFORMATION * dataPtr;
	char newChoice[50];

	while(head != 0)
	{
		dataPtr = head -> dataPTR;
		head = head -> next;

		if(strcmp(dataPtr->username, userChoice) == 0)
		{
			printf("Your choice has been found!!!\n");
			printf("Your searched for username: %s\n", dataPtr->username);
			printf("Your searched for password: %s\n", dataPtr->password);

		}

		/*
		 * These extra if elses handle what happens when the head reaches
		 * a node that does not match our choice
		 */
		else if(strcmp(dataPtr->username, userChoice) > 0)
		{

			printf("Username too short.\n");
		}

		else if(strcmp(dataPtr->username, userChoice) < 0)
		{
			printf("Username too long.\n");
		}
		

		
	}

}

/*
 * Writes data to the file. It is extermely inefficient, but semi-works. 
 */

void writeToFileAfterExiting(INFORMATION * newInfoPTR1, INFORMATION * newInfoPTR2, INFORMATION * newInfoPTR3, INFORMATION * newInfoPTR4, INFORMATION * newInfoPTR5, INFORMATION * newInfoPTR6)
{
	
	FILE *fileOfClasses;

	fileOfClasses = fopen("exam3.txt", "w");

	//fprintf(fileOfClasses, "%s\n %s\n %s\n %s\n %s\n", newInfoPTR1, newInfoPTR2, newInfoPTR3, newInfoPTR4, newInfoPTR5);
	/*
	 * This is actually really inefficient, which is why I tried to do the bottom function, but I realized too late
	 * so Ill just go with this since it semi-works
	 */

	fprintf(fileOfClasses, "%s\n %s\n %s\n %s\n %s\n %s\n %s\n %s\n %s\n %s\n %s\n %s\n", newInfoPTR1->username, newInfoPTR1->password, newInfoPTR2->username, newInfoPTR2->password, newInfoPTR3->username, newInfoPTR3->password, newInfoPTR4->username, newInfoPTR4->password, newInfoPTR5->username, newInfoPTR5->password, newInfoPTR6->username, newInfoPTR6->password);


	fclose(fileOfClasses);	




}

/*
 * Function was meant to be more efficient
 * than above while trying to utilize fwrite()
 * instead fprintf(), but realized this too late. 
 *
 */

void writeToFile(NodeType * head)
{
	
	/*
	FILE * fileOfClasses;

	fileOfClasses = fopen("exam3.txt", "w");

	INFORMATION * dataPTR;

	while(head != 0)
	{
		dataPTR = head -> dataPTR;

		
		//fwrite(dataPTR, sizeof(*dataPTR), 1, fileOfClasses);

		head = head-> next;

	}

	fclose(fileOfClasses);
	*/

}






