#### 堆排序
###### 1.基本概念
**堆：** 堆是一棵完全二叉树，这是他的结构性
**最大堆：** 每个节点的值都大于或等于其左右孩子节点的值，这是他的堆序性
**最小堆：** 每个节点的值都小于或等于其左右孩子节点的值，这是他的堆序性

###### 2.基本思想
堆排序就是把需要排序的序列构建最大堆或者最小堆，此时，最大值或者最小值就在堆顶位置，然后把该元素放于最后，对剩下的序列再一次构造堆，知道完成排序

堆排序的时间复杂度为 O(nlogn)，由于要构造堆，因此不适用于序列个数较少的情况。

堆排序是一种不稳定的排序
###### 3. 实现方式：
**1.辅助数组：**
构建堆完成后将最大或最小元素放于一个辅助数组，再重新构造堆

**2.无需辅助数组：**
构建堆完成后将最大或最小元素放于堆末端
再重新构造堆
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413170153944.gif)

```java
public class SelectSort {

	/**
	 * 获取左孩子的下标
	 * 
	 * 父亲：(i-1)/2
	 * 左孩子：2*i+1
	 * 右孩子：2*i+2
	 * 
	 * @param i
	 * @return
	 */
	private static int leftChild(int i) {
		return (2 * i) + 1;
	}
	

	/**
	 * 交换
	 * @param a
	 * @param fromIndex
	 * @param toIndex
	 */
	private static void swapReference(int[] a,int fromIndex,int toIndex) {
		
		int tmp = a[fromIndex];
		a[fromIndex] = a[toIndex];
		a[toIndex] = tmp;
	}
	
	/**
	 * 重建堆（下滤）
	 * @param a
	 * @param i
	 * @param n
	 */
	private static void percDown(int[] a,int i,int n) {
		
		int child;
		//保存父节点值
		int tmp;
		//父节点和左右孩子比较，找出三者最大值做父节点建最大堆
		for(tmp = a[i];leftChild(i)<n;i = child) {
			child = leftChild(i);
			//左孩子<右孩子 ，child指向右孩子
			if(child != n-1 && a[child]<a[child+1]) {
				child++;
			}
			//孩子比父节点大，则交换
			if (tmp<a[child]) {
				a[i] = a[child];
			}
			//父节点最大则跳出循环
			else {
				break;
			}
		}
		//完成交换
		a[i] = tmp;
	}
	
	/**
	 * 堆排序
	 * 
	 * 1.建堆
	 * 2.删除最大节点（交换到数组（堆）末端）
	 * 3.重建堆
	 * 
	 * @param a
	 */
	public static void heapSort(int[] a) {
		
		for(int i = a.length/2-1;i>=0;i--) {
			percDown(a, i, a.length);
		}
		for(int i = a.length-1;i>0;i--) {
			swapReference(a,0,i);
			percDown(a, 0, i);
		}		
	}
	
	public static void main(String[] args) {
		int[] a = {1,8,2,6,4,2,3,8,4,6,10,12,45,21,31};
		System.out.println(a.length);
		heapSort(a);
		for(int i = 0;i<a.length;i++) {
			System.out.print(a[i]+"\t");
		}
	}
	
}

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190413164339550.png)