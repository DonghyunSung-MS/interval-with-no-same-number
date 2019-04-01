//# interval-with-no-same-number
//Algorithms - parametric search

#include <stdio.h>

const int MAX=100000;
int n;
int arr[MAX];

//input n and array of n
//output max r : that can make interval with no same numbers
//ex) 10
// 1 3 1 2 4 2 1 3 2 1
//         | | | |
//r
//1 is always possible
//3 is possible to make the interval
//4 can
//5 not

//check n whether is possible -> all should different num
void merging(int interval[],int s1,int e1,int s2,int e2){
  int p=s1;
  int q=s2;
  int tmp[MAX];
  int inx=0;
  
  while(p<=e1&&q<=e2){
    if(interval[p]>interval[q]){
      tmp[inx++]=interval[q];
      q++;
    }
    else{
      tmp[inx++]=interval[p];
      p++;
    }
  }
  if(p>e1){
    for(int i=q;i<=e2;i++) tmp[inx++]=interval[i];
  }
  else{
    for(int i=p;i<=e1;i++)  tmp[inx++]=interval[i];
  }
  for(int i=s1;i<=e2;i++) interval[i]=tmp[i-s1];
}
void mergesort(int interval[],int s,int e){
  if(s>=e) return;
  else{
    int mid=(s+e)/2;
    mergesort(interval,s,mid);
    mergesort(interval,mid+1,e);
    merging(interval,s,mid,mid+1,e);
  }
}

int check(int arr[],int r){
  int inx=0;
  int interval[MAX];
  int cnt;
  while(inx<=n-r){
    cnt=0;
    for(int i=0;i<n;i++){
      interval[i]=arr[i];
    }
    mergesort(interval,inx,inx+r-1);
    for(int i=inx;i<inx+r-1;i++){
      if(interval[i]==interval[i+1]) break;
      else cnt++;
    }
    if(cnt>=r-1) return 1;
    inx++;
  }
  return 0;
}

int main(){
  scanf("%d",&n);
  for(int i=0;i<n;i++)  scanf("%d",&arr[i]);
  //before Parametric search check the end
  if(check(arr,n)==1){
    printf("%d",n);
    return 0;
  }
  
  int start=1;
  int end=n;
  while(start+1<end){
    int mid=(start+end)/2;
    // mid is possible to make interval
    if(check(arr,mid)==1) start=mid;
    // mid can not make it
    else end=mid;
  }
  printf("%d\n",start);
  //printf("%d\n",check(arr,4));
  //printf("%d\n",check(arr,3));
  //printf("%d\n",check(arr,5));
  return 0;
  
}
