### Python接口使用demo

此处将示例如何利用C#和Python两种语言协同进行接口开发，示例功能为选出属性值在某一范围内的数字化对象（`DGObject`），并将其图标和数据行高亮。该demo目前仅支持`Borehole`类型下对`top`属性的筛选。由于需要传入参数，该demo适合在`IronPython Console`中调用。

#### 1. API格式

API调用格式如下：

```csharp
toolDemo.test(type,attribute,down,up)
```



#### 2. 传入参数

**type**：`Domain`类型，目前只支持`Borehole`

**attribute**:  筛选的属性，目前只支持`Borehole`的`top`属性

**down**：下限

**up**:  上限



#### 3. 开发过程

##### 3.1 编写C#调用接口

此处我们将在C#程序里添加两个功能，`getDGObject() `和` selectObject() `函数以供Python调用。娴熟Python者也可以尝试纯Python开发出自己的工具。

`getDGObjec() `函数添加在`DGOjects.cs`文件中，该函数作用是以数组形式返回`DGOjects`里所包含的所有`DGOject`。代码如下：

```csharp
//in DGOjects.cs
public DGObject[] getDGObject(){
            int size=_objs.Count;
            DGObject[] objs = new DGObject[size];
            _objs.Values.CopyTo(objs, 0);
            return objs;   

        }
```

`selectObject()`  函数添加在`IS3View.cs`文件中，该函数用于选取并高亮指定的数字化对象`DGObject`。代码如下：

```csharp
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
            args.addedObjs.Add(objs.FirstOrDefault().parent.definition.Name, objs);
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
# IS3View.cs: public void selectObject(String type,DGObject obj)    
# DGOjects.cs: public DGObject[] getDGObject()  
# Relative Classes
# IS3View.cs, DGOjects.cs, Domain.cs, MainFrame.xaml.cs
# Relative Functions
# public async Task<bool> selectByPoint(Point screenPoint)   IS3View.cs
# public void objSelectionChangedListener(object sender,ObjSelectionChangedEventArgs e)  MainFrame.xaml.cs

def test(type,attribute,down,up):
    if(down>up):	# tranform into lower case
        tmp=up
        up=down
        down=tmp
    attr=attribute.lower()
    tp=type.lower()

    if(tp=="borehole"):	# 'Borehole' type
        domain=is3.prj.getDomain(DomainType.Geology)
        objs=domain["Allboreholes"] 
        obj=objs.getDGObject()   # list of DGObject
        if(attr=="top"):	# 'top' attribute
            for i in obj:
                if((i.Top<=up)and(i.Top>=down)):
                    is3.mainframe.views[0].selectObject("Borehole",i)
        else:
            print("No such attribute")
```

**4. 查看效果**

完成后将在`IronPython Console`里对该demo的实现效果进行测试。

#### 4. 测试用例

在`IronPython Console`面板里输入以下代码：

```python
>>> import toolDemo
>>> toolDemo.test('borehole','top',2.7,2.75)
```


#### 5. 测试结果

测试效果如下图：

<div style= text-align:center>
<img src=".\test.png"  style='width:600px'; 'left: 50%'/>
</div>

由上图可见，符合范围内的`Borehole`类型的数字化对象在地图和**Data View**上均被高亮，且在**Object View**面板中被展示。
