#include <stdio.h>

#include <stdlib.h>

#include <string.h>

 

#define MAX_CANDIDATES 100

 

// Defining a structure to represent a candidate

struct Candidate {

    char name[50];

    int votes;

};

 

// Defining a queue to store the voters' choices

struct Queue {

    int front, rear, size;

    unsigned capacity;

    int* array;

};

 

// creating a new queue

struct Queue* createQueue(unsigned capacity) {

    struct Queue* queue = (struct Queue*)malloc(sizeof(struct Queue));

    queue->capacity = capacity;

    queue->front = queue->size = 0;

    queue->rear = capacity - 1;

    queue->array = (int*)malloc(queue->capacity * sizeof(int));

    return queue;

}

 

// checking if the queue is full

int isFull(struct Queue* queue) {

    return (queue->size == queue->capacity);

}

 

// checking if the queue is empty

int isEmpty(struct Queue* queue) {

    return (queue->size == 0);

}

 

// adding an element to the queue

void enqueue(struct Queue* queue, int item) {

    if (isFull(queue)) {

        printf("Queue is full, cannot enqueue.\n");

        return;

    }

    queue->rear = (queue->rear + 1) % queue->capacity;

    queue->array[queue->rear] = item;

    queue->size++;

}

 

// removing an element from the queue

int dequeue(struct Queue* queue) {

    if (isEmpty(queue)) {

        printf("Queue is empty, cannot dequeue.\n");

        return -1;

    }

    int item = queue->array[queue->front];

    queue->front = (queue->front + 1) % queue->capacity;

    queue->size--;

    return item;

}

 

int main() {

    int numCandidates, numVoters;

  

    printf("Enter the number of candidates: ");

    scanf("%d", &numCandidates);

 

    if ( numCandidates<0 || numCandidates > MAX_CANDIDATES  ) {

        printf("invalid number of candidates. Maximum allowed is %d.\n", MAX_CANDIDATES);

        return 1;

    }

 

    struct Candidate candidates[MAX_CANDIDATES];

 

    for (int i = 0; i < numCandidates; i++) {

        printf("Enter the name of candidate %d: ", i + 1);

        scanf("%s", candidates[i].name);

        candidates[i].votes = 0;

    }

 

    printf("Enter the number of voters: ");

    scanf("%d", &numVoters);

   

    if ( numVoters<0 || numVoters>1000  ) {

        printf("invalid number of voters.");

        return 1;

    }

 

    struct Queue* voteQueue = createQueue(numVoters);

    int choice;

    for (int i = 1; i <= numVoters; i++) {

        printf("Voter %d, please select your candidate (1-%d): ", i , numCandidates );

        scanf("%d", &choice);

 

        if ( choice <= 0 || choice >= numCandidates   ) {

            printf("Invalid choice. Please choose a valid candidate.\n");

            --i; // Decrement to allow the same voter to vote again

            continue;

        }

         enqueue(voteQueue, choice);

       

       

    }

 

    // Counting the votes

    while (!isEmpty(voteQueue)) {

        int choice = dequeue(voteQueue);

        candidates[choice].votes++;

    }

 

    printf("Election Results:\n");

    for (int i = 0; i < numCandidates; i++) {

        printf("%s: %d votes\n", candidates[i].name, candidates[i].votes);

    }

 

    return 0;

}
