# mustafa98
//Readers Writer problem (reader's prioirty)
#include<stdio.h>
#include<pthread.h>
#include<semaphore.h>
sem_t reader;
sem_t writer;
int data=1;
int readers=0;
void *wfunc(void *n)
{
int num=((int) n);
printf("\nwriter %d entered the block and waiting for its turn\n",num);
sem_wait(&writer);
printf("\nwriter %d is ready to write\n",num);
data=data*2;
printf("\nThe Data %d is written by writer %d\n",data,num);
sleep (2);
printf("\nwriter %d is now leaving the block\n",num);
sem_post(&writer);
}
void *rfunc(void * n)
{
int num= ((int ) n);
sem_wait (&reader);
readers++;
if (readers==1)
{
printf ("\nREADER %d Blocked The writers\n",num);
sem_wait(&writer);
}
sem_post(&reader);
printf("\nReader %d read data-> %d\n",num,data);
sleep(2);
sem_wait(&reader);
readers--;
if(readers==0)
{
printf("\nwriters are unblocked\n");
sem_post (&writer);
}
sem_post(&reader);
}

void  main()
{
int num[5];
for   (int i=0;i<5;i++)
{
num[i]=i;
}
pthread_t wthread[5];
pthread_t rthread[5];
sem_init(&reader,0,1);
sem_init(&writer,0,1); 
for   (int i=0;i<5;i++)
{
pthread_create(&wthread,NULL,wfunc,(void *)num[i]);
pthread_create(&rthread,NULL,rfunc,(void *)num[i]);
}

for   (int i=0;i<5;i++)
{
pthread_join(&wthread,NULL);
pthread_join(&rthread,NULL);
}
}
