# Round-Robin
#include<stdio.h>
#include<conio.h>
void main(){
	int t_bt[15],f,q,n,i,wt[15],tat[15],p_id[15],bt[15],t[15],te;
	double avg_wt,avg_tat;
	te=0;
	avg_wt=0;
	avg_tat=0;
	printf("Enter the number of processes (max 15): ");
	scanf("%d",&n);
	printf("\n\n");
	printf("Enter the quantum number: ");
	scanf("%d",&q);
	printf("\n\n");
	for(i=0;i<n;i++){
		p_id[i]=i;
		printf("Enter the burst time of process %d:\t",i);
		scanf("%d",&bt[i]);
		t_bt[i]=bt[i]; //storing the burst time of process in a temporary array
		if(bt[i]<q){ //Eg: if bt[i]=1 and q=2 => t[i]=1
			t[i]=1;
		}
		else{
			if(bt[i]%q==0){ //Eg: if bt[i]=4 and q=2 => t[i]=2
				t[i]=bt[i]/q;
			}
			else{ //Eg: if bt[i]=3 and q=2 => t[i]=1+1=2
				t[i]=(bt[i]/q)+1;
			}
		}
	}
	L:for(i=0;i<n;i++){
		if(t[i]>0){
			if(t_bt[i]>=q){
				t_bt[i]=t_bt[i]-q;
				--t[i];
				te=te+q;
			}
			else{
				te=te+t_bt[i];
				t_bt[i]=0;
				--t[i];
			}
			if(t[i]==0){ //check whether the computuation of process 'i' is completed, if it is completed, then,
				wt[i]=te-bt[i]; //waiting time of that process is calculated.
			}
		}
	}
	f=0;
	for(i=0;i<n;i++){ //it will count the number of processes whose computation is completed.
		if(t[i]==0){ 
			f++; //'f' is the number of processes whose computation is completed.
		}
	}
	if(f!=n){ //if the computation of all the process is not completed, then
		goto L; //the remaining process will be computed until the computation of all the process is completed.
	}
	for(i=0;i<n;i++){
		tat[i]=wt[i]+bt[i];
		avg_wt=avg_wt+wt[i];
		avg_tat=avg_tat+tat[i];
	}
	avg_wt=avg_wt/n;
	avg_tat=avg_tat/n;
	printf("\n\nProcess id\tBurst time\tWaiting time\tTurn around time\n");
	for(i=0;i<n;i++){
		printf("%d\t\t%d\t\t%d\t\t%d\n",p_id[i],bt[i],wt[i],tat[i]);
	}
	printf("\n\nAverage waiting time is: %lf\n",avg_wt);
	printf("Average turn around time is: %lf",avg_tat);
}
