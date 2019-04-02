### 循环结构

>for

	for(int i=0;i<10;i++){
		System.out.println("输出10次");
	}
	
>for语句的特殊用法

	1.表达式1位置内容为空时
		int sum=0;
		int i=1;
		for(;i<=10;i++){
			sum+=i;
		}
		System.out.println("1到10的和为:"+sum);
		
	2.表达式3位置内容为空时
		int sum=0;
		for(int i=1;i<=10;){
			sum+=i;
			i++;
		}
		System.out.println("1到10的和为:"+sum);

	3.死循环
		for( ; ; ){
			System.out.println("死循环");
		}

	4.表达式1，3可以用逗号隔开，写多个表达式
		for(int i=1,j=6;i<6;i+=2,j-=2){
			System.out.println("i,j="+i+","+j);
		}
		结果：
			i,j=1,6
			i,j=3,4
			i,j=5,2
			
>break(用于循环，可使程序终止循环而执行循环后面的语句)
	
	int sum=0;
	for(int i=1;i<=100;i++){
		if(sum>=4000){  //在某种条件下提前结束
			break;
		}
		sum+=i;
	}

>continue(跳过循环体中剩余语句而进入下一次循环，只能用于循环中)

	统计总和时，跳过所有个位为3的
	int sum=0;
	for(int i=1;i<=100;i++){
		if(i%10==3){
			continue;
		}
		sum+=i;
	}





