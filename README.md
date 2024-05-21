STACK
#include <limits.h>
#include <stdio.h>
#include <stdlib.h>

struct StackNode {
    int data;
    struct StackNode* next;
};


struct Stack {
    struct StackNode* top;
    unsigned size; 
    unsigned capacity; 
};


struct StackNode* newNode(int data)
{
    struct StackNode* stackNode = (struct StackNode*)malloc(sizeof(struct StackNode));
    stackNode->data = data;
    stackNode->next = NULL;
    return stackNode;
}


struct Stack* createStack(unsigned capacity)
{
    struct Stack* stack = (struct Stack*)malloc(sizeof(struct Stack));
    stack->root = NULL;
    stack->size = 0; 
    stack->capacity = capacity; 
    return stack;
}


int isEmpty(struct Stack* stack)
{
    return stack->size == 0;
}

int isFull(struct Stack* stack)
{
    return stack->size == stack->capacity;
}

void push(struct Stack* stack, int data)
{
    if (isFull(stack)) {
        printf("\tOverflow! Cannot push %d to stack\n", data);
        return;
    }
    struct StackNode* stackNode = newNode(data);
    stackNode->next = stack->top;
    stack->top = stackNode;
    stack->size++; 
    printf("%d pushed to stack\n", data);
}

int pop(struct Stack* stack)
{
    if (isEmpty(stack)) {
        printf("\tUnderflow!\n");
        return INT_MIN;
    }
    struct StackNode* temp = stack->top; 
    stack->top = stack->top->next; 
    int popped = temp->data; 
    free(temp); 
    stack->size--; 

    return popped; 
}

int peek(struct Stack* stack)
{
    if (isEmpty(stack)) {
        printf("\tUnderflow!\n");
        return INT_MIN;
    }
    return stack->top->data; 
}


void show(struct Stack* stack)
{
    struct StackNode* current = stack->root;
    printf("\nElements present in the stack: \n");
    while (current != NULL) {
        printf("%d\n", current->data); 
        current = current->next; 
    }
    printf("Current stack size: %u\n", stack->size);
}


int main()
{
    unsigned capacity;
    printf("Welcome to Stack Creator\n");
    printf("Please provide the stack's capacity: ");
    scanf("%u", &capacity);

    struct Stack* stack = createStack(capacity);
    int choice;

    while (1)
    {
        printf("\nPerform operations on the stack:");
        printf("\n1. Push the element\n2. Pop the element\n3. Show\n4. Peek\n5. End");
        printf("\n\nEnter the choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            {
                int item;
                printf("Give an element to add: ");
                scanf("%d", &item);
                push(stack, item); 
            }
            break;
        case 2:
            printf("%d popped from stack\n", pop(stack));
            break;
        case 3:
            show(stack); 
            break;
        case 4:
            printf("%d is the element on the top of the stack\n", peek(stack)); 
            break;
        case 5:
            exit(0);
        default:
            printf("\nInvalid choice!!");
        }
    }

    return 0;
}
