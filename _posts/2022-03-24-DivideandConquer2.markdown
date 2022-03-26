## Divide-and-Conquer

___
### 색종이 만들기

```markdown
아래의 그림에서 만들어진 파란색 색종이의 개수와 하얀색 색종이의 개수를 구하는 문제이다.
아래의 그림에서 종이의 길이는 8이고 종이를 분할해 만들 색종이의 길이는 1이다.
```
![image](https://media.vlpt.us/images/kpg0518/post/38fd896f-4a00-42fb-a62f-148b2ab1abac/image.png)

위의 문제를 분할정복 알고리즘으로 해결하는 방법은 먼저 주어진 종이를 가로, 세로로 이등분한다.

![image](https://blog.kakaocdn.net/dn/cTJinj/btqystWcjjz/VfGvcTTV2MK8zNKqEMfBDk/img.png)

![image](https://bn1301files.storage.live.com/y4mp_2FOPOskDsZpBgKmBauWoOImGkb2DA6CKdUf7iZmX4e2T4sWxUxTM6JByK7XSFCVRz5QlXwZXU4TFPAD0Byhq6kE5bJuz1kuNmJubGodCHu3QgP2kvqgSvwqxi3pFVhRaQS9UlDqgD6Ng2JDRWjjb3Mv9mXBvrqOO0B1QrNn5bXV6lg_z55Ghhp3qq7yu5H0UxqVv7eLASw4ZW_cvnpawGeWJB9JbKXAo501SCkuDs?encodeFailures=1&width=110&height=94)

위와 같이 분할했을 때 한가지의 색만 존재하지 않으면 다시 종이를 분할 한다

![image](https://bn1301files.storage.live.com/y4mYTMqbMxaztAKgXU28hq9hQJ-v7WS7tqnvBP362tXkphJCiACegywEImnwP1t8ORlbAgIUqJBMs8iiRQKq64NcibGSGKYLvqHMWR0bUiKF3nWMaHumEWDFMe4mTb4CLJT9p4xT_5wzjCFW2RlyYPP9fOyD68a6vr9t05_BGKERPw03MLdYoZ9LLgdFsGxPXmfSuVfqA47ycEAQopMOe02trukCkxi66YYANBmyOtnyzk?encodeFailures=1&width=91&height=82)

종이에 한가지의 색만 존재한다면 리턴.

> 이렇게 종이를 4개로 똑같이 분할하여 종이에 같은 색이 나올 때까지 분할한다. 분할 하다가 한 등분에 모두 같은 색만 있다면 리턴시킨다. 이를 반복해 함수가 모두 종료될때까지 시행한다.
> 이해하기 쉽게 정리하면 다음과 같다.

![image](https://bn1301files.storage.live.com/y4mFKgfDyzmxlg5vOaRenqfNFeJEKzgS2oN5uGPDarSFmOUTQRjUIlU75p0zaWcUanEL5dXgDxM248I9yrm03X_C23bcNCzaLE02cm6DEqyIIjBw1t57KWrUxzLQZKAFLx9Tqod1FVHTMQfZy7SzOQL-Whiv3SzWz-tAVvFRLBcHD1lneW6rgoiyP1IEAM5tZwpj1Mw_W2lYE-J4oGUIcm4JjJUOp5R3cQEPjuPioNSelM?encodeFailures=1&width=734&height=827)

이 문제를 java코드로 작성하면
```markdown
package 구나연;
import java.util.Scanner;

public class DivideandConquer2 {
	
	
	public static int white = 0;
	public static int blue = 0;
	public static int[][] board;
 
	public static void main(String[] args) {
		
		Scanner in = new Scanner(System.in);
		
		int N = in.nextInt();
		
		board = new int[N][N];
		
		for(int i = 0; i < N; i++) {
			for(int j = 0; j < N; j++) {
				board[i][j] = in.nextInt();
			}
		}
		
		partition(0, 0, N);
		
		System.out.println(white);
		System.out.println(blue);
		
	}
	
	public static void partition(int row, int col, int size) {
		
	
		if(colorCheck(row, col, size)) {
			if(board[row][col] == 0) {
				white++;
			}
			else {
				blue++;
			}
			return;
		}
		
		int newSize = size / 2;	
		
		partition(row, col, newSize);						
		partition(row, col + newSize, newSize);				
		partition(row + newSize, col, newSize);			
		partition(row + newSize, col + newSize, newSize);	
	}
	
	
	
	public static boolean colorCheck(int row, int col, int size) {
	
		int color = board[row][col];	
		
		for(int i = row; i < row + size; i++) {
			for(int j = col; j < col + size; j++) {
				
				if(board[i][j] != color) {
					return false;
				}
			}
		}
	
		return true;
	}
}
```
