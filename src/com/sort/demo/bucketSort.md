### 一，桶排序
#### 1.算法原理
* 1.桶排序核心思想就是将要排序的数据分到几个有序的桶里，每个通在分别进行排序，每个桶排序完成后再把每个桶里的数据按照顺序依次取出，组成新的序列，该序列就是排好序的序列。类似归并排序中中的分治思想。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417113208841.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTkyMjI4OQ==,size_16,color_FFFFFF,t_70)
*  
#### 2.算法分析
* 时间复杂度接近O(N)，所以说桶排序是线性时间排序
* 空间复杂度：桶排序中，需要创建M个桶的额外空间，以及N个元素的额外空间，所以桶排序的空间复杂度为 O(N+M)
* 桶排序是稳定排序，但是，如果桶内的排序是选择快速排序，那么桶排序是不稳定的

#### 3.算法实现
```java
public class BucketSort {

	
		/**
		 * 桶排序
		 * @param data
		 * @param min	min的值为0
		 * @param max	max的值为待排序数组中最大值+1
		 */
	  	public static void bucketSort(int[] data, int min, int max) {  
	    
			// 缓存数组  
		    int[] tmp = new int[data.length];  
		    // buckets用于记录待排序元素的信息  
		    // buckets数组定义了max-min个桶  
		    int[] buckets = new int[max - min];  
		    // 计算每个元素在序列出现的次数  
		    for (int i = 0; i < data.length; i++) {  
		        buckets[data[i] - min]++;  
		    }  
		    // 计算“落入”各桶内的元素在有序序列中的位置  
		    for (int i = 1; i < max - min; i++) {  
		        buckets[i] = buckets[i] + buckets[i - 1];  
		    }  
		    // 将data中的元素完全复制到tmp数组中  
		    System.arraycopy(data, 0, tmp, 0, data.length);  
		    // 根据buckets数组中的信息将待排序列的各元素放入相应位置  
		    for (int k = data.length - 1; k >= 0; k--) {  
		         data[--buckets[tmp[k] - min]] = tmp[k];  
		    }  
		}
	 
	  
		/**
		 * 获取待排序数组中的最大值
		 * @param array
		 * @return
		 */
		private static int getMax(int[] array){
			
			int max = array[0];
			for(int i = 1;i<array.length;i++) {
				if (array[i] > max) {
					max = array[i];
				}
			}
			return max;
		}
		
		/**
		 * 获取待排序数组的最小值
		 * @param array
		 * @return
		 */
		private static int getMin(int[] array){
			
			int min = array[0];
			for(int i = 1;i<array.length;i++) {
				if (array[i] < min) {
					min = array[i];
				}
			}
			return min;
		}
		
	   /**
		 * 利用类似散列的结构解决桶排序
		 * @param data
		 * @param min
		 * @param max
		 */
		public static void bucketSort3(int[] data,int min,int max) {
			
			System.out.println(max+"\t"+min);
			
			//构造桶
			//桶数量
			int bucketNum = (max - min) / data.length + 1;
			//每个桶是一个ArrayList
			ArrayList<ArrayList<Integer>> bucketArr = new ArrayList<>(bucketNum);
			//相当于有一个横向数组，每一个横向数组下标是一个ArrayList的散列结构
			for(int i = 0;i<bucketNum;i++) {
				 bucketArr.add(new ArrayList<Integer>());
			}
			
			 //将每个元素放入桶
		    for(int i = 0; i < data.length; i++){
		    	//元素所在的横向数组下标，根据下标进入指定的ArrayList,解决重复数据问题
		        int num = (data[i] - min) / (data.length);
		        bucketArr.get(num).add(data[i]);
		    }
		    
		    //对每个桶进行排序
		    for(int i = 0; i < bucketArr.size(); i++){
		        Collections.sort(bucketArr.get(i));
		    }
		   
		    System.out.println(bucketArr.toString());
		}
		
	  
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			int[] a = {1,8,2,6,4,2,3,8,4,6,10,12,45,21,31};
			System.out.println(a.length);
			bucketSort(a, 0, getMax(a)+1);
			for(int i = 0;i<a.length;i++) {
				System.out.print(a[i]+"\t");
			}
			bucketSort3(a,getMin(a),getMax(a));
		}

}

```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417112537834.png)

### 二，计数排序

#### 1.算法原理
计数排序是一种非基于比较的排序算法，其空间复杂度和时间复杂度均为O(n+k)线性时间复杂度，其中k是整数的范围。基于比较的排序算法时间复杂度最小是O(nlogn)的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041713104913.gif)

#### 2.算法实现
```java
		/**
		 * 计数排序
		 * @param array
		 */
		private static void countSort(int[] data) {
			
			// 得到最大最小值
			int max = getMax(data);
			int min = getMin(data);
			// 得到辅助数组来存储data数组每一元素出现的次数
			// data数组中每一个数和最小值的差是一定的，这一步有排序的作用
			int[] help = new int[max - min + 1];
			for (int i = 0; i < data.length; i++) {
				int mapPos = data[i] - min;
				help[mapPos]++;
			} 
			// 查看help数组
			System.out.println(Arrays.toString(help));
			int k = 0;
			// 得到输出数组
			for (int i = 0; i < help.length; i++) {
				//根据help数组中存的次数去取排列好的元素
				while (help[i] != 0) {
					data[k] = min + i;
					k++;
					help[i]--;
				}
			}
		}
```