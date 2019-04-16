# sleeping-barber-problem
void barber(void *arg)/*Barber Process*/
{
	while(time(NULL)<end_time || count>0)
	{
		sem_wait(&customers);/*P(customers)*/            
		sem_wait(&mutex);/*P(mutex)*/
		count--;
		printf("Barber:cut hair,count is:%d.\n",count);
		sem_post(&mutex);/*V(mutex)*/
		sem_post(&barbers);/*V(barbers)*/
		sleep(3);       
	}
}
void customer(void *arg)/*Customers Process*/
{
	while(time(NULL)<end_time)
	{
		sem_wait(&mutex);/*P(mutex)*/
		if(count<N)
		{
			count++;
			printf("Customer:add count,count is:%d\n",count);
			sem_post(&mutex);/*V(mutex)*/
			sem_post(&customers);/*V(customers)*/
			sem_wait(&barbers);/*P(barbers)*/
		}
		else
			/*V(mutex)*/
			/*If the number is full of customers,just put the mutex lock let go*/
			sem_post(&mutex);
		sleep(1);
	}
}
