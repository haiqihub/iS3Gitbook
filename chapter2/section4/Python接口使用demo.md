### Python接口使用demo

此处将示例如何利用C#和Python两种语言协同进行接口开发，示例功能为选出属性值在某一范围内的数字化对象（DGObject），并将其图标和数据行高亮。该demo仅支持Borehole类型下对top属性的筛选。由于需要传入参数，该demo适合在IronPython Console中调用。

#### 1. API格式

API调用格式如下：

```
toolDemo.test(type,attribute,down,up)
```



#### 2. 传入参数

*type*：Domain类型，目前只支持Borehole

*attribute*:  筛选的属性，目前只支持Borehole的top属性

*down*：下限

*up*:  上限



#### 3. 开发过程

##### 3.1 编写C#调用接口

此处我们将在C#程序里添加两个功能，getDGObject() 和 selectObject() 函数以供Python调用。娴熟Python者也可以尝试纯Python开发出自己的工具。

getDGObjec() 函数添加在DGOjects.cs文件中，该函数作用是以数组形式返回DGOjects里所包含的所有DGOject。代码如下：

```c#
//in DGOjects.cs
public DGObject[] getDGObject(){
            int size=_objs.Count;
            DGObject[] objs = new DGObject[size];
            _objs.Values.CopyTo(objs, 0);
            return objs;   

        }
```

selectObject()  函数添加在IS3View.cs文件中，该函数用于选取并高亮指定的数字化对象DGObject。代码如下：

```c#
//in IS3View.cs
public void selectObject(String type,DGObject obj){
            if (Globals.isThreadUnsafe())
            {
                Globals.application.Dispatcher.Invoke(new Action(()=>
                    {
                        selectObject(type,obj);
                    }));
                return;
            }
            if(type=="Borehole"){
                IS3GraphicsLayer glayer=getLayer(type) as IS3GraphicsLayer;
                glayer.highlightObject(obj,true);
            }
            ObjSelectionChangedEventArgs args=new ObjSelectionChangedEventArgs();
            args.addedObjs=new Dictionary<string, IEnumerable<DGObject>>();
            List<DGObject> objs=new List<DGObject>();
            objs.Add(obj);
            args.addedObjs.Add(type,objs);
            objSelectionChangedTrigger(this,args);       
        }
```

##### 3.2 重新生成dll

在C#里添加了自己的代码后，需要重新生成解决方案，更新动态链接库。

##### 3.3 编写Python接口

在此，我们将调用上述补充的C#接口，完成自定义demo。代码如下：

```python
import is3
from is3 import DomainType,Globals

# Changes
# IS3View.cs: public void selectObject(String type,DGObject obj)    line:890
# DGOjects.cs: public DGObject[] getDGObject()  line:124
# ---------------------------------------------------------------------------------------------
# Relative Classes
# IS3View.cs, DGOjects.cs, Domain.cs, MainFrame.xaml.cs
# Relative Functions
# public async Task<bool> selectByPoint(Point screenPoint)   IS3View.cs
# public void objSelectionChangedListener(object sender,ObjSelectionChangedEventArgs e)  MainFrame.xaml.cs

def test(type,attribute,down,up):
    if(down>up):
        tmp=up
        up=down
        down=tmp
    attr=attribute.lower()
    tp=type.lower()

    if(tp=="borehole"):
        domain=is3.prj.getDomain(DomainType.Geology)
        objs=domain["Allboreholes"] 
        obj=objs.getDGObject()   
        obj=objs.values()
        if(attr=="top"):
            for i in obj:
                if((i.Top<=up)and(i.Top>=down)):
                    is3.mainframe.views[0].selectObject("Borehole",i)
        else:
            print("No such attribute")
            
```

**4. 查看效果**

完成后将在IronPython Console里对该demo的实现效果进行测试。



#### 4. 测试用例

在IronPython Console面板里输入以下代码：

```python
>>> import toolDemo
>>> toolDemo.test('borehole','top',2.7,2.75)
```



#### 5. 测试结果

测试效果如下图：

<img src=".\test.png" alt="test" style="zoom: 40%;" />



由上图可见，符合范围内的Borehole类型的数字化对象在地图和Data View上均被高亮，且在Object View面板中被展示。