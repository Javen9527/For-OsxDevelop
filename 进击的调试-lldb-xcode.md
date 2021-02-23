### 多线程调试 ###

代码如下：

        #include <iostream>
        #include <thread>
        #include <string>
        #include <mutex>
        using namespace std;

    mutex mu;
    static int number = 0;

    void threadFunc(string s)
    {
        {
            lock_guard<mutex> locker(mu);
            cout << "This is in " << s << " thread: " << endl;
        }

        while(true)
        {
            lock_guard<mutex> locker(mu);
            cout << s << ":" << ++number <<endl;
            std::this_thread::sleep_for(chrono::milliseconds (100));
        }
    }

    void mainFunc(string s)
    {
        {
            lock_guard<mutex> locker(mu);
            cout << "This is in " << s << " thread: " << endl;
        }

        while(true)
        {
            lock_guard<mutex> locker(mu);
            cout << s << ":" << ++number <<endl;
            std::this_thread::sleep_for(chrono::milliseconds (100));
        }
    }

    int main()
    {
        cout << "======== Hello debugger start! ========\n" << endl;

        // thread
        thread t1(threadFunc, "t1");
        t1.detach();

        thread t2(threadFunc, "t2");
        t2.detach();

        // main
        mainFunc("main");

        cout << "\n======== Hello debugger start! ========" << endl;
        return 0;
    }


![image](https://github.com/Javen9527/For-OsxDevelop/blob/main/pic/debug1.png)
