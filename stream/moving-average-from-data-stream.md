# Moving Average from Data Stream

[642. Moving Average from Data Stream](https://www.lintcode.com/problem/moving-average-from-data-stream/?_from=ladder&&fromId=116)

#### 題目

Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

Example Example 1:

```text
MovingAverage m = new MovingAverage(3);
m.next(1) = 1 // return 1.00000
m.next(10) = (1 + 10) / 2 // return 5.50000
m.next(3) = (1 + 10 + 3) / 3 // return 4.66667
m.next(5) = (10 + 3 + 5) / 3 // return 6.00000
```

#### 解法 1 \( 會超時 \)

```cpp
class MovingAverage {
private:
    int count = 0;
    int windowSize;
    vector<int> datas;
    int oldestTag = 0;
public:
    /*
    * @param size: An integer
    */MovingAverage(int size) {
        windowSize = size;
        datas = vector<int>(size, 0);
    }

    /*
     * @param val: An integer
     * @return:  
     */
    double next(int val) {
        if(count >= windowSize){
            datas[oldestTag] = val;
            oldestTag = (oldestTag < datas.size() - 1) ? oldestTag + 1 : 0;
        }else{
            datas[count] = val;
            count++;
        }

        double sum = 0;
        for(int i=0; i < datas.size(); i++){
            sum += datas[i];
        }
        return sum / count;
    }
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param = obj.next(val);
 */
```

#### 解法 2 \( 不用 queue \)

```cpp
class MovingAverage {
private:
    int count = 0;
    int windowSize;
    vector<int> datas;
    int oldestTag = 0;
    double sum = 0;
public:
    /*
    * @param size: An integer
    */MovingAverage(int size) {
        windowSize = size;
        datas = vector<int>(size, 0);
    }

    /*
     * @param val: An integer
     * @return:  
     */
    double next(int val) {
        if(count >= windowSize){
            sum -= datas[oldestTag]; 
            datas[oldestTag] = val;
            oldestTag = (oldestTag < datas.size() - 1) ? oldestTag + 1 : 0;
        }else{
            datas[count] = val;
            count++;
        }
        sum += val;

        return sum / count;
    }
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param = obj.next(val);
 */
```

#### 解法 3 - Queue

```cpp
class MovingAverage {

private:
    std::queue<int> queue;
    int windowSize = 0;
    double sum = 0;
public:
    /*
    * @param size: An integer
    */MovingAverage(int size) {
        // do intialization if necessary
        windowSize = size;
    }

    /*
     * @param val: An integer
     * @return:  
     */
    double next(int val) {
        // write your code here
        sum += val;
        if(queue.size() == windowSize){
            sum -= queue.front();
            queue.pop();
        }
        queue.push(val);

        return sum / (queue.size());
    }
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param = obj.next(val);
 */
```

