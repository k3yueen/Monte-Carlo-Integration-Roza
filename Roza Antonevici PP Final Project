#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<math.h>
#include<omp.h>

double MonteCarlo()
{
	int nthreads;
	printf("Enter Number of Threads for execution: ");//Enter total number of threads you want to execute//
	scanf("%d",&nthreads);
	
	omp_set_num_threads(nthreads);//Setting the number of threads you initialized//

	int N;
	printf("Enter Number of Iterations: ");//Enter total number of iterations to get the nearest pi value the greater the value the closer the pi value//
	scanf("%d",&N);

	//Setting algorithms values//
	int i=0,count=0;
	double x,y,z;

	
	double start_time = omp_get_wtime();//Getting the starting time for computing the parallel or sequential time//

	#pragma omp parallel
	{
		unsigned int seed = omp_get_thread_num();//Getting the seed value by using the unique id of each threads//
		//Note rand_r is a thread safe random number generator if you use another function besides it the speedup wont be gained//

		#pragma omp for reduction(+:count) firstprivate(x,y,z)
		for(i=0;i<N;i++)
		{
			x = (double)rand_r(&seed)/RAND_MAX;//Rand function to get the random value of x coordinate//
			y = (double)rand_r(&seed)/RAND_MAX;//Rand function to get the random value of y coordinate//

			z = ((x*x) + (y*y));//calculate the z axis of the given radius//

			if(z<=1)//if z is less then or equal to 1 then increment the count as it is in the corresponding area of the circle//
				count++;
		}

	}

	double end_time = omp_get_wtime();//Getting the ending time for computing the parallel or sequential time//

	if(nthreads==1)
		printf("Sequential Time = %f\n",end_time-start_time);
	else
	{
		printf("Parallel Time = %f\n",end_time-start_time);
		double sequential_time = 17.683732;//This sequential time is taken when N = 1000000000
		printf("Speedup = %fx\n",sequential_time/(end_time-start_time));//Speedup gained by dividing sequential time by parallel time//
	}
	return (((double)count / (double)N)*4.0);//Returning the pi value//
}

int main()
{
	omp_set_dynamic(0);//Setting the dynamic optimization to false so that we can use the maximum threads on the system//
	double pi = MonteCarlo();
	printf("Approximate PI value: %f\n",pi);

	return 0;
}
