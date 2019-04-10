# os-project
Q6. Write a program for multilevel queue scheduling algorithm. There must be three queues generated. There must be specific range of priority associated with every queue. Now prompt the user to enter number of processes along with their priority and burst time. Each process must occupy the respective queue with specific priority range according to its priority. Apply Round Robin algorithm with quantum time 4 on queue with highest priority range. Apply priority scheduling algorithm on the queue with medium range of priority and First come first serve algorithm on the queue with lowest range of priority. Each and every queue should get a quantum time of 10 seconds. CPU will keep on shifting between queues after every 10 seconds.
------------------------------------------------------------------------------------------------------------------------------
#include<stdio.h>
#include<iostream>
using namespace std;
void result();
struct Processes
{
    int processid;
    int priority;
    int burstT;
};
int main()
{
    int i,y,j,k[3];
    cout<<"Priority range of Lowest\t: 0-3\n";
    cout<<"Priority range of Middle\t: 4-6\n";
    cout<<"Priority range of Highest\t: 7-9\n\n";
    int n;
    cout<<"Enter total number processes:";

    cin>>n;
    int p[n],burst[n];
    struct Processes obj1[n],obj2[n],obj3[n];
    for(int i=0;i<n;i++)
    {
        cout<<"Process ID : "<<(i+1)<<"\nEnter Priority of process :";
        

        cin>>p[i];
        cout<<"Enter Burst Time of process :";
        
        cin>>burst[i];
    }
    k[1]=0;k[2]=0;k[3]=0;
    for(int i=0;i<n;i++)
    {
        if(p[i]<=3&&p[i]>=0)
        {
            obj1[k[1]].processid=i+1;
            obj1[k[1]].priority=p[i];
            obj1[k[1]].burstT=burst[i];
            k[1]++;
        }
        else if(p[i]>3&&p[i]<=6)
        {
            obj2[k[2]].processid=i+1;
            obj2[k[2]].priority=p[i];
            obj2[k[2]].burstT=burst[i];
            k[2]++;
        }
        else if(p[i]>6&&p[i]<=9)
        {
            obj3[k[3]].processid=i+1;
            obj3[k[3]].priority=p[i];
            obj3[k[3]].burstT=burst[i];
            k[3]++;
        }
    }
    cout<<"---------------------------------------------------------------";
    cout<<"\nLowest prior Queue \t(FIFO)\n";
    if(k[1]==0)
        cout<<"No process in this priority range 0-3\n\n";
    else
    {
        for(int j=0;j<k[1];j++)
        {
            cout<<obj1[j].processid<<"\t"<<obj1[j].priority<<"\t"<<obj1[j].burstT<<"\n";
        }
    }
    cout<<"---------------------------------------------------------------";
    cout<<"\nMiddle prior Queue \t(Priority Sheduling)\n";
    if(k[2]==0)
        cout<<"No process in this priority range 4-6\n\n";
    else
    {
        for(int j=0;j<k[2];j++)
        {
            cout<<obj2[j].processid<<"\t"<<obj2[j].priority<<"\t"<<obj2[j].burstT<<"\n";
        }
    }
    
    cout<<"---------------------------------------------------------------";
    cout<<"\nHighest prior Queue (Round Robin scheduling)\n";
    if(k[3]==0)
        cout<<"No process in this priority range 7-9"<<endl<<endl;
    else
    {
        for(int j=0;j<k[3];j++)
        {
            cout<<obj3[j].processid<<"\t"<<obj3[j].priority<<"\t"<<obj3[j].burstT<<"\n";
        }
    }
    int o=0;
    int m,t=0,imp=0,Queuecount=0,Qsize=0,flag[k[1]],flag1[k[2]],flag2[k[3]];
    int Q[3];
    for(int m=0;m<3;m++)
    {
        Q[m]=1;
    }
    for(int m=0;m<k[1];m++)
    {
        flag[m]=1;
    }
    for(int m=0;m<k[2];m++)
    {
        flag1[m]=1;
    }
    for(int m=0;m<k[3];m++)
    {
        flag2[m]=1;
    }
    cout<<"\n\nRESULT :\n\n";
    while(t<100)
    {
        if(Q[imp]==0)
        {
            for(int m=0;m<3;m++)
            {
                if(Q[m]==0)
                {
                    Queuecount++;
                }
            }
            if(Queuecount==3)
            {
                break;
                break;
            }
        }
        else{
            switch(imp)
            {
                case 0:
                {
                    int count=0;
                    int pro=0;
                    int e=t+10;
                    if(k[1]==0){
                        Q[0]=0;
                        break;
                    }
                    else{
                        while(t<=e)
                        {
                            if(flag[pro]==0)
                            {
                                for(int m=0;m<k[1];m++)
                                {
                                    if(flag[m]==0)
                                    {
                                        count++;
                                    }
                                }
                                if(count==k[1])
                                {
                                    Q[0]=0;
                                    break;
                                }
                            }
                            else
                            {
                                int QTime=10;
                                int remtime=obj1[pro].burstT;
                                if((t+remtime)>e && remtime>0)
                                {
                                    int h=t+remtime;
                                    remtime=h-e;
                                    obj1[pro].burstT=remtime;
                                    cout<<"\tp"<<obj1[pro].processid<<" Process from "<<t<<"---"<<e<<"\n";
                                    t=e;
                                    break;
                                }
                                else if(remtime<10)
                                {
                                    cout<<"\tp"<<obj1[pro].processid<<" Process from "<<t<<"---"<<(t+remtime)<<"\n";
                                    flag[pro]=0;
                                    t+=remtime;
                                    obj1[pro].burstT=0;
                                }
                                else if(remtime>10)
                                {
                                    cout<<"\tp"<<obj1[pro].processid<<" Process from "<<t<<"---"<<(t+10)<<"\n";
                                    t=t+10;
                                    obj1[pro].burstT-=10;
                                }
                            }
                            pro++;
                            pro=pro%k[1];
                            
                        }
                        
                    }
                    break;
                }
                case 1:
                {
                    struct Processes c;
                    for(int u=0;u<k[2];u++)
                    {
                        for(int r=u;r<k[2];r++)
                        {
                            if(obj2[u].priority>obj2[r].priority)
                            {
                                c=obj2[u];
                                obj2[u]=obj2[r];
                                obj2[r]=c;
                            }
                        }
                    }
                    int count=0;
                    int e=t+10;
                    int pro1=0;
                    if(k[2]==0){
                        Q[1]=0;
                    }
                    else{
                        while(t<=e)
                        {
                            if(flag1[pro1]==0)
                            {
                                for(int m=0;m<k[2];m++)
                                {
                                    if(flag1[m]==0)
                                    {
                                        count++;
                                    }
                                }
                                if(count==k[2])
                                {
                                    Q[1]=0;
                                    break;
                                    break;
                                }
                            }
                            else
                            {
                                int QTime=10;
                                int remtime=obj2[pro1].burstT;
                                if(t+remtime>e && remtime>0)
                                {
                                    int h=t+remtime;
                                    remtime=h-e;
                                    obj2[pro1].burstT=remtime;
                                    cout<<"\tp"<<obj2[pro1].processid<<" Process from "<<t<<"---"<<e<<"\n";
                                    t=e;
                                    break;
                                    
                                }
                                
                                else if(remtime<=10)
                                {
                                    cout<<"\tp"<<obj2[pro1].processid<<" Process from "<<t<<"---"<<(t+remtime)<<"\n";
                                    flag1[pro1]=0;
                                    t+=remtime;
                                    obj1[pro1].burstT=0;
                                }
                                else if(remtime>10)
                                {
                                    cout<<"\tp"<<obj2[pro1].processid<<" Process from "<<t<<"---"<<(t+10)<<"\n";
                                    obj2[pro1].burstT-=10;
                                    t=t+10;
                                }
                                
                            }
                            pro1++;
                            pro1=pro1%k[2];
                        }
                    }
                    
                    break;
                }
                case 2:
                {
                    int count=0;
                    int e=t+10;
                    int pro2=0;
                    if(k[3]==0){
                        Q[2]=0;
                    }
                    else{
                        while(t<=e)
                        {
                            if(flag2[pro2]==0)
                            {
                                for(int m=0;m<k[3];m++)
                                {
                                    if(flag2[m]==0)
                                    {
                                        count++;
                                    }
                                }
                                if(count==k[3])
                                {
                                    Q[2]=0;
                                    break;
                                    break;
                                }
                            }
                            else
                            {
                                int timeq=4;
                                int remtime=obj3[pro2].burstT;
                                if((t+remtime)>e && remtime>0 && remtime<=4)
                                {
                                    int h=t+remtime;
                                    remtime=h-e;
                                    obj3[pro2].burstT=remtime;
                                    cout<<"\tp"<<obj3[pro2].processid<<" Process from "<<t<<"---"<<e<<"\n";
                                    t=e;
                                    break;
                                }
                                else if(remtime<=timeq)
                                {
                                    cout<<"\tp"<<obj3[pro2].processid<<" Process from "<<t<<"---"<<(t+remtime)<<"\n";
                                    t+=remtime;
                                    obj3[pro2].burstT=0;
                                    flag2[pro2]=0;
                                }
                                else
                                {
                                    cout<<"\tp"<<obj3[pro2].processid<<" Process from "<<t<<"---"<<(t+timeq)<<"\n";
                                    obj3[pro2].burstT-=timeq;
                                    t+=timeq;
                                }
                            }
                            pro2++;
                            pro2=pro2%k[3];
                        }
                    }
                    break;
                }
                    
                    
            }
            
        }
        imp++;
        imp=imp%3;
    }
    
    
    return 0;
}

