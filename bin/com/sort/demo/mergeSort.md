### 一，归并排序
#### 归并排序算法实现
**算法思路：**
如果要排序一个数组，我们先从数组中间把数组分成左数组和右数组两部分，分别对左右数组进行排序，然后将排序好的数组合并成结果数组，排序就完成了，最后只需将结果数组复制回原数组即可。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417110436651.gif)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416150852955.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTkyMjI4OQ==,size_16,color_FFFFFF,t_70)

**核心思想** ----- 分治思想

分治也即是分而治之，将一个大问题分解为小的子问题来解决。分治算法一般都是用递归来实现的。分治是一种解决问题的处理思想，递归是一种编程技巧。

**归并排序分析**
1. 稳定排序算法
2. 归并排序的执行效率与原始数据的有序程度无关，其时间复杂度是非常稳定的，不管是最好情况、最坏情况，还是平均情况，时间复杂度都是O(NlogN)。
3. 归并排序有一个缺点，那就是它不是原地排序算法。在进行子数组合并的时候，我们需要临时申请一个数组来暂时存放排好序的数据。因为这个临时空间是可以重复利用的，因此归并排序的空间复杂度为O(n)，最多需要存放n个数据。

**具体实现**
```java
public class MergeSort {

	/**
	 * 归并排序   O(NlogN)
	 * 
	 * 思想：分治
	 * @param oldArray
	 * @param tmpArray
	 * @param left
	 * @param right
	 */
	private static void mergeSort(int[] oldArray,int[] tmpArray,int left,int right) {
		if(left < right) {
			//中间元素下标
			int center = (left + right)/2;
			//分治左边数组[left , center]
			mergeSort(oldArray, tmpArray, left,center);
			//分治右边数组[center+1 , right]
			mergeSort(oldArray, tmpArray, center+1, right);
			//分开排序后合并成一个新数组在copy会原数组，此时原数组已经有序
			merge(oldArray, tmpArray, left, center+1,right);
			
		}
	}
	
	/**
	 * 排序，合并
	 * @param oldArray
	 * @param tmpArray
	 * @param leftStart
	 * @param rightStart
	 * @param rightEnd
	 */
	private static void merge(int[] oldArray, int[] tmpArray, int leftStart, int rightStart, int rightEnd) {
		
		//左数组开始下标
		int leftEnd = rightStart - 1;
		//辅助数组起始下标
		int tmpPos = leftStart;
		//数组元素数量
		int numElements = rightEnd - tmpPos + 1;
		
		//左右数组进行比较,按序放入辅助数组
		while(leftStart <= leftEnd && rightStart <= rightEnd) {
			if(oldArray[leftStart] <= oldArray[rightStart]) {
				tmpArray[tmpPos++] = oldArray[leftStart++];
			}else {
				tmpArray[tmpPos++] = oldArray[rightStart++];
			}
		}
		
		//判断子数组是否还有元素，有则拷贝到 tmp 后面
		while(leftStart <= leftEnd) {
			tmpArray[tmpPos++] = oldArray[leftStart++];
		}
		
		//判断子数组是否还有元素，有则拷贝到 tmp 后面
		while(rightStart <= rightEnd) {
			tmpArray[tmpPos++] = oldArray[rightStart++];
		}
		
		//复制辅助数组回原数组
		for(int i = 0;i<numElements;i++,rightEnd--) {
			oldArray[rightEnd] = tmpArray[rightEnd];
		}
	}


	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int[] a = {1,8,2,6,4,2,3,8,4,6,10,12,45,21,31};
		int[] tmpArray = new int[a.length];
		mergeSort(a, tmpArray, 0, a.length-1);
		for(int i = 0;i<a.length;i++) {
			System.out.print(a[i]+"\t");
		}
		
	}

}
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190416151336991.png)
