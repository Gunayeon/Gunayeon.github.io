## Divide-and-Conquer

___
### Closest Pair(최근접 점의 쌍)

![image](https://img1.daumcdn.net/thumb/R300x0/?fname=https://k.kakaocdn.net/dn/KMZYE/btqJDYLqx1u/VbJW5UVdIysK11nHFYb9I0/img.png)

위와 같이 평면상에 존재하는 모든 점의 거리 중 가장 가까운 두 점이 무엇인 지 구하려면 위의 그럼처럼 점의 개수가 20개인 가정하에 190(총 20개의 원소 중 2개를 선택하는 조합)번의 거리를 구해야 한다. 그러나 이 문제를 분할정복 알고리즘을 이용하면 더 간단한 문제로 변하게된다.


평면상의 점들을 두부분으로 나누면, 아래의 그림과 같이 되는데

![Image](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRXlYKDYZoc2msL7X5crOkFe-U6TcVqxRPSa9zsef5iwJF9vyYe4YgUCl7oOFoUAfiPRiU&usqp=CAU)

20개의 점들을 10개씩 나눈 다음에 각 평면에 대해 최소점을 구하게 된다면 한 평면당 45개의 두 점사이의 거리를 구할 수 있게 되고, 이를 4개의 평면으로 줄이게 되면 총 20번의 연산을 해야 한다. 그러나 여기서 추가로 고려해야 할 상황은 서로 다른 평면에 존재하는 점에서 최근접 점이 발생하는 경우이다.

![Image](https://post-phinf.pstatic.net/MjAyMDA5MjVfMTMx/MDAxNjAxMDM5NzY2NDYw.e_XcSRSoy056cLctFzPc1F6aVyn__SW1L47iSuJzCLEg.3E3HfVIVqsrLdCzN5ZpNMiq2j-wtbYuZTjk-8acAMc4g.PNG/image.png?type=w1200)

이와 같은 상황을 고려하게 되면, 각 평면에서 두 점사이의 최소 거리를 구한 다음 두 평면이 맞닿은 방향의 가장자리로 부터 최소거리인 10이내의 점들 간의 간격을 구해야 한다. 좌측의 한 점과 우측의 한 점을 순차적으로 선택하여 10보다 짧은 두 점을 구하면 된다. 좌측엔 5개의 점 우측엔 4개의 점 총 20번의 추가적인 연산을 해주면 된다.

이와 같은 알고리즘을 Java코드로 구현하게 되면 다음과 같다.
```markdown
package 구나연;
import java.util.*;

public class DivideandConquer {
	
	static int [][] position;

	public static void main(String[] args) {
		// TODO Auto-generated method stub
    Scanner sc =new Scanner(System.in);
    
    int N=sc.nextInt();//점의 개수
    
    position = new int[N][2];//각 점의 위치
    
    //위치입력
    for(int i=0;i<N;i++) {
    	position[i][0]=sc.nextInt();//x값 위치
    	position[i][1]=sc.nextInt();//y값 위치
    	
    }
    //x중앙값 구하기
    int[] xArray=new int[N];
    for(int i=0;i<N;i++) {
    	xArray[i]=position[i][0];
    }
    double xMed=Xmedian(xArray);
    //평면 2분할
    int firstNod=-1;
    int secondNod=-1;
    ArrayList<Integer>Divide1=new ArrayList<Integer>();
    ArrayList<Integer>Divide2=new ArrayList<Integer>();
    
    for(int i=0;i<N;i++) {
    	if(position[i][0]<=xMed) {
    		Divide1.add(i);
    	}
    	else {
    		Divide2.add(i);
    	}
    }
    //각 분할에서 최소값 선정
    double min1=1000;
    int node11=-1;
    int node12=-1;
    for(int i=0;i<Divide1.size()-1;i++) {
    	for(int j=i+1;j<Divide1.size();j++) {
    		int a=Divide1.get(i);
    		int b=Divide1.get(j);
    		double tmp=calDist(a,b);
    		if(tmp<min1) min1=tmp; node11=2; node12=b;
    		}
    }
    //각 분할에서 최소값 선정
    double min2=1000;
    int node21=-1;
    int node22=-1;
    for(int i=0;i<Divide1.size()-1;i++) {
    	for(int j=i+1;j<Divide1.size();j++) {
    		int a=Divide1.get(i);
    		int b=Divide1.get(j);
    		double tmp=calDist(a,b);
    		if(tmp<min2) min1=tmp; node21=2; node22=b;
    		}
    }
    //두 분할 중 더 최소값을 선정
    double falsemin=0;
    if(min1<=min2) {
    	falsemin=min1; firstNod=node11; secondNod=node12;
    }
    else {
    	falsemin=min2;firstNod=node21;secondNod=node22;
    }
    
    //중앙 부분 산정
    int medianXmin=(int)(xMed-falsemin)-1;
    int medianXmax=(int)(xMed-falsemin)+1;
    ArrayList<Integer>DivideMed=new ArrayList<Integer>();
    for(int i=0;i<N;i++) {
    	if((position[i][0]>medianXmin)&&(position[i][0]<medianXmax)) {
    		DivideMed.add(i);
    	}
    }
    
    //중앙 부분의 최소값 산정
    double realmin=falsemin;
    for(int i=0;i<DivideMed.size()-1;i++) {
    	for(int j=i+1;j<DivideMed.size();j++) {
    		int a=DivideMed.get(i);
    		int b=DivideMed.get(j);
    		double tmp=calDist(a,b);
    		if(tmp<realmin) {
    			realmin=tmp; firstNod=a; secondNod=b;
    		}
    	}
    }
    System.out.println(realmin+" "+firstNod+" "+secondNod);
	}
	//두점 사이의 거리를 구하는 공식
	public static double calDist(int a, int b) {
		return Math.sqrt(Math.pow(position[a][0]-position[b][0],2)+Math.pow(position[a][1]-position[b][1],2));
	}
	//x중앙값 구하기
	public static double Xmedian(int[] xArray) {
		Arrays.sort(xArray);
		double answer=0;
		
		int size=xArray.length;
		//x 값이 짝수라면
		if(size%2==0) {
			int m=size/2;
			int n=(size/2)-1;
			answer=(double)(xArray[m]-xArray[n])/2;
		}
		else {
			int m=size/2;
			answer=xArray[m];
		}
		return answer;
		
	}

}
  ```
