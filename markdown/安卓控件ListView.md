#ListVIew

<h2>本次我们实现了一个类似网易新闻的界面</h2>

>  <b>总体思路</b>


1.写布局ListView
2.找到ListVIew
3.封装新闻数据到list集合中，目的是为了adapter提供数据展示
4.创建一个adapter继承BaseAdapter 写一个构造方法接受list 集合数据，复写四个方法

**a.创建一个构造方法
b.封装getCount方法**
<b>c.getView方法：</b>
　　　1.写模板代码
　　　　　 如果不为空 将一个布局文件装换为view对象返回 
> view=View.inflater(Context context,int resuorceId,VIewGroup root)

　　　&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;2.找到view上的这些子控件，目的是将list集合的数据对应的设置给这些子控件
　　　3.从list集合中获取postion条目上要显示的数据
　　　4.将获取的数据设置给这些控件

d.getItem:将list集合中指定的postion的对象返回
e.getItemId:直接返回postion

５.创建一个封装的Adapter对象，设置给listView
6.设置点击listview条目的点击事件


#代码
##构造listview
```java
这里只提供关键代码
//第四步的4.创建一个adapter继承BaseAdapter 写一个构造方法接受list 集合数据，复写四个方法

public class ListCreat extends BaseAdapter{//继承BaseAdapter
    Context content ;
    ArrayList<Data> arrayList = null;
    public ListCreat(Context context,ArrayList<Data> arrayList) {
        super();
        this.content = context;
        this.arrayList = arrayList;//获取存放数据的集合
    }

    @Override
    public int getCount() {
        return arrayList.size();//显示的条目数
    }

    @Override
    public Object getItem(int position) {//返回Data数据
        return arrayList.get(position);
    }

    @Override
    public long getItemId(int position) {
        return position;//返回id
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View view =null;
        if(convertView!=null){
            view = convertView;
        }
        else {
               view = View.inflate(content, R.layout.lsit_view, null);
            // 返回一个view，里面用自己的布局文件填充,root的话就是包装一些就写null
        }
        TextView biaoti = (TextView) view.findViewById(R.id.biaoti);
        TextView neirong = (TextView) view.findViewById(R.id.neirong);
        ImageView pic = (ImageView) view.findViewById(R.id.pic);
        Data data = arrayList.get(position);
      	//获取数据对象
        pic.setImageDrawable(data.pic);
        biaoti.setText(data.title);
        neirong.setText(data.neirong);
	//给屏幕上的各个条目赋值
        return view;
    }
```
##存放数据的Data和操作数据的DataUtil
```java
public class DataUtil {

//这里是伪造的新闻数据

public class Data {
    public String title;
    public String dec;
    public Drawable pic;
    public String url;
}
//这里是操作数据的部分
    static public ArrayList<Data> getallnews(Context context){
        ArrayList<Data> arrayList = new ArrayList<>();
        for(int i=0;i<100;i++)
        {

            Data data = new Data();
            data.title="王小鸡";
            data.dec="王小鸡帅又帅";
            data.url="https://www.baidu.com";
            data.pic= ContextCompat.getDrawable(context, R.drawable.x);
           //这里重点学习了如何获取图片
           //ContextCompat 是推荐的取代方式 记住他
            arrayList.add(data);


            Data data1 = new Data();
            data1.title="xie";
            data1.dec="王小鸡xxx";
            data1.url="https://www.baidu.com";
            data1.pic= ContextCompat.getDrawable(context, R.drawable.y);
            arrayList.add(data1);


            Data data2 = new Data();
            data2.title="王小鸡111";
            data2.dec="王小鸡11111";
            data2.url="https://www.baidu.com";
            data2.pic= ContextCompat.getDrawable(context, R.drawable.z);
            arrayList.add(data2);

        }
        return arrayList;
    }
}




```

##最后是主体函数的实现部分

```java
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        context = this;
        ListView lv = (ListView) findViewById(R.id.lv);
        ArrayList<Data> arrayList = DataUtil.getallnews(context);
        CreatListView creatListView = new CreatListView(context,arrayList);
        lv.setAdapter(creatListView);//设置点击时间
        lv.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Data data =  (Data) parent.getItemAtPosition(position);
                Intent intent = new Intent();//创建一个奴隶
                intent.setAction(Intent.ACTION_VIEW);//让他去打开界面
                intent.setData(Uri.parse(data.url));//构造一个url的链接
                startActivity(intent);//奴隶开始工作
            }
        });
```


##总结

> 主体来说，这次要注意的还有这几点
如何转换图片到Drawable ContextCompat.getDrawable(context, R.drawable.y);
如何将一个布局文件填充到view

(1)view = View.inflate(content, R.layout.lsit_view, null);
   (2)         view =LayoutInflater.from(context).inflate(R.layout.buju_listview,null);
       (3)     LayoutInflater layoutInflater = (LayoutInflater)context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            view=layoutInflater.inflate(R.layout.buju_listview,null);
            
            
            
            
            
当然个人推荐第一种