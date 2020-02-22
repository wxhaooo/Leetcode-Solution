# 157-Simple: Read N Characters Given Read4

## 考点

* 字符串拼接
* 字符串拷贝

```cpp
// Forward declaration of the read4 API.
int read4(char *buf);

class Solution {
public:
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    int read(char *buf, int n) {
        int cur_n = 0;
        std::string buf_tmp;
        while(cur_n<n){
            int read_num = read4(buf);
            if(read_num == 0)
                return cur_n;
            buf_tmp += std::string(buf).substr(0,read_num);
            strcpy(buf,buf_tmp.c_str());
            if(cur_n + read_num >= n)
                return n;
            else
                cur_n +=read_num;
        }
        return cur_n;
    }
};
```
