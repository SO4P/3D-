# 1、简答题

+ 解释 游戏对象（GameObjects） 和 资源（Assets）的区别与联系。

游戏对象：游戏中的每个对象都是一个游戏对象，有自己的属性，像是资源的一种实例。
资源：游戏中可以用到的资源，可以被多个游戏对象使用。


+ 下载几个游戏案例，分别总结资源、对象组织的结构
    + Assets
        + Content  存放资源，预制的地方
        + Plugins  存放C#等代码的地方

+ 编写一个代码，使用 debug 语句来验证 MonoBehaviour 基本行为或事件触发的条件

        using System.Collections;
        using System.Collections.Generic;
        using UnityEngine;
        public class NewBehaviourScript1 : MonoBehaviour {
            void Awake()
            {
                Debug.Log("Awake!");
                }
            void Start()
            {
                Debug.Log("Start!");
            }
            void Update()
            {
                Debug.Log("Update!");
            }
            void FixedUpdate()
            {
                Debug.Log("FixedUpdate!");
            }
            void LateUpdate()
            {
                Debug.Log("LateUpdate!");
            }
            void Reset()
            {
                Debug.Log("Reset!");
            }
            void OnGUI()
            {
                Debug.Log("onGUI!");
            }
            void OnDisable()
            {
                Debug.Log("onDisable!");
            }
            void OnDestroy()
            {
                Debug.Log("onDestroy!");
            }
        }
运行结果：
<img width="163" alt="1" src="https://user-images.githubusercontent.com/37209772/37970687-a148dd2c-31c3-11e8-9a93-1fa0fd94bc9d.png">
<img width="162" alt="2" src="https://user-images.githubusercontent.com/37209772/37970698-a669b6d2-31c3-11e8-9b84-325130b8c758.png">
结束：
<img width="160" alt="3" src="https://user-images.githubusercontent.com/37209772/37970722-b0daa414-31c3-11e8-8125-df709d957193.png">


+ 查找脚本手册，了解GameObject,Transform,Component 对象

    + 分别翻译官方对三个对象的描述
        + GameObject：Unity场景中所有物体的基类
        + Transform：对象的位置、旋转（角度）和大小
        + Component：GameObject所有附加物的基类 
    + 描述下图中 table 对象（实体）的属性、table 的 Transform 的属性、 table 的部件
        
        table 的对象是 GameObject，第一个选择框是 activeSelf 属性，第二个选择框是Transform属性，第三个选择框是Mesh Filter筛网过滤器属性，第四个选择框是Box Collider属性，第五个选择框是Mesh Renderer筛网渲染器属性，第六个选择框是Default-Material属性。
    + 用 UML 图描述 三者的关系
<img width="329" alt="9" src="https://user-images.githubusercontent.com/37209772/37971694-19f22240-31c6-11e8-95b0-d7588837db5a.png">


+ 整理相关学习资料，编写简单代码验证以下技术的实现：
    + 查找对象
    
            GameObject table;
            table = GameObject.Find("table");
            if(table != null)
            {
                Debug.Log("table");
            }
<img width="162" alt="4" src="https://user-images.githubusercontent.com/37209772/37970741-bc353b3a-31c3-11e8-8034-10e6e13ed434.png">

    + 添加子对象
    
            GameObject temp = GameObject.CreatePrimitive(PrimitiveType.Cube);
            temp.transform.parent = table.transform;
            temp.transform.position = new Vector3(2, 1, 1);
<img width="105" alt="5" src="https://user-images.githubusercontent.com/37209772/37970748-c23928e8-31c3-11e8-906c-ae95b660862c.png">

    + 遍历对象树
    
            foreach (Transform child in transform)
            {
                Debug.Log(child.position);
            }
<img width="161" alt="6" src="https://user-images.githubusercontent.com/37209772/37970756-c7913a2e-31c3-11e8-8f4c-c8e430657bbe.png">

    + 清楚所有子对象
    
            foreach (Transform child in transform)
            {
                Destroy(child.gameObject);
            }
<img width="158" alt="7" src="https://user-images.githubusercontent.com/37209772/37970769-cf441f5c-31c3-11e8-8d1e-dbe7a71bcec1.png">
    
    
+ 资源预设（Prefabs）与 对象克隆 (clone)
    + 预设（Prefabs）有什么好处？
    
        预设是一个可以重复使用的模板，可以在短时间复制大量相同属性的对象而且不用通过大量代码来实现，操作简单，节省时间。
    + 预设与对象克隆 (clone or copy or Instantiate of Unity Object) 关系？

        预设只需修改预设体的属性就能改变所有实例化对象的属性，而克隆不行
    + 制作 table 预制，写一段代码将 table 预制资源实例化成游戏对象
    
            GameObject table = (GameObject)Resources.Load("table");
            Instantiate(table);
+ 尝试解释组合模式（Composite Pattern / 一种设计模式）。使用 BroadcastMessage() 方法
    + 向子对象发送消息

    parent:
    
        void Start () {
            this.BroadcastMessage("print","name:");
        }
        
    son:
    
        void print(string n)
        {
            Debug.Log(n + this.gameObject.name);
        }
    
运行结果：
<img width="160" alt="8" src="https://user-images.githubusercontent.com/37209772/37970791-dca961fc-31c3-11e8-826c-f8b7e4834ecb.png">

#2、编程实验

代码如下：

    using System.Collections;
    using System.Collections.Generic;
    using UnityEngine;

    public class main : MonoBehaviour {

        private bool[] check = new bool[9];  //用来储存各个位置是否有棋子
        private int color = 1;  //代表颜色，1为蓝色，2为红色
        private int[] play = new int[9];  //用来储存各个位置棋子颜色
        private int count = 0; //用来计算棋子数
        private Texture2D[] chess = new Texture2D[9];  //用来储存各个位置棋子状态
        public Texture2D Blue;  //蓝色棋子
        public Texture2D Red;  //红色棋子
        // Use this for initialization
        void Start () {
            clear();
        }
	
	    // Update is called once per frame
	    void Update () {
		
	    }

        private void OnGUI()
        {
            bool che = false;
            Change();  //更新棋盘

            //9个button代表9个格子
            if (GUI.Button(new Rect(200, 20, 80, 80),chess[0]) && !check[0])
            {
                check[0] = true;
                play[0] = color;
                che = true;
            }
            if (GUI.Button(new Rect(200, 100, 80, 80), chess[1]) && !check[1])
            {
                check[1] = true;
                play[1] = color;
                che = true;
            }
            if (GUI.Button(new Rect(200, 180, 80, 80), chess[2]) && !check[2])
            {
                check[2] = true;
                play[2] = color;
                che = true;
            }
            if (GUI.Button(new Rect(280, 20, 80, 80), chess[3]) && !check[3])
            {
                check[3] = true;
                play[3] = color;
                che = true;
            }
            if (GUI.Button(new Rect(280, 100, 80, 80), chess[4]) && !check[4])
            {
                check[4] = true;
                play[4] = color;
                che = true;
            }
            if (GUI.Button(new Rect(280, 180, 80, 80), chess[5]) && !check[5])
            {
                check[5] = true;
                play[5] = color;
                che = true;
            }
            if (GUI.Button(new Rect(360, 20, 80, 80), chess[6]) && !check[6])
            {
                check[6] = true;
                play[6] = color;
                che = true;
            }
            if (GUI.Button(new Rect(360, 100, 80, 80), chess[7]) && !check[7])
            {
                check[7] = true;
                play[7] = color;
                che = true;
            }
            if (GUI.Button(new Rect(360, 180, 80, 80), chess[8]) && !check[8])
            {
                check[8] = true;
                play[8] = color;
                che = true;
            }

            if (che)  //放下一个棋子
            {
                if (color == 1)
                {
                    color = 2;
                }
                else
                {
                    color = 1;
                }
                count++;
                if(Win() != 0)  //游戏结束
                {
                    for(int i = 0;i < 9; i++)
                    {
                        check[i] = true;
                    }
                }
                switch (Win())  //输出游戏结束时的状态
                {
                    case 1:
                        Debug.Log("Blue Win!");
                        break;
                    case 2:
                        Debug.Log("Red Win!");
                        break;
                    case 3:
                        Debug.Log("Draw");
                        break;
                    default:
                        break;
                }
            }
            if (GUI.Button(new Rect(480, 100, 80, 40), "ReStart"))  //重置按钮
            {
                clear();
            }
        }

        private int Win()  //判断游戏是否结束
        {
            for (int i = 0; i < 3; i++)
            {
                if (play[3 * i + 0] == play[3 * i + 1] && play[3 * i + 1] == play[3 * i + 2])
                    return play[3 * i + 0];
                else if (play[i + 0] == play[i + 3] && play[i + 3] == play[i + 6])
                    return play[i + 0];
            }
            if(play[0] == play[4] && play[4] == play[8])
            {
                return play[4];
            }
            else if(play[6] == play[4] && play[4] == play[2])
            {
                return play[4];
            }
            for (int i = 0; i < 9; i++)
            {
                if (play[i] == 0)
                    break;
                else if (i == 8)
                    return 3;
            }
            return 0;
        }

        private void clear()  //清空棋盘
        {
            for (int i = 0; i < 9; i++)
            {
                check[i] = false;
                play[i] = 0;
                chess[i] = null;
            }
            count = 0;
            color = 1;
        }

        private void Change()  //决定显示的棋子
        {
            for(int i = 0;i < 9; i++)
            {
                if(play[i] != 0)
                {
                    if (play[i] == 1)
                        chess[i] = Blue;
                    else if (play[i] == 2)
                        chess[i] = Red;
                }
            }
        }
    }
