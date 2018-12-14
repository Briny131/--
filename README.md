# 拖拽


    通过逻辑，dom结构分离，来实现拖拽标签嵌套分离

对象结构如下：
```
function Tuo(){
    this.children=[]
    this.pos=[]
    this.ele=''
}
```

插入操作函数：
```
function Insert(tree,str,L,T){
    if(tree.children.length==0){
        str.ele.remove()
        str.ele.style.width=str.ele.style.height='50%'
        str.ele.style.position=''
        del(template,str)
        tree.children.push(str)
        tree.ele.append(str.ele)
        str.pos=[str.ele.getBoundingClientRect().top,str.ele.getBoundingClientRect().top+str.ele.offsetHeight,str.ele.getBoundingClientRect().left,str.ele.getBoundingClientRect().left+str.ele.offsetLeft+str.ele.offsetWidth]
    }else{
        let j=1
        for(let i of tree.children){
            if(L>i.pos[2]&&L<i.pos[3]&&T>i.pos[0]&&T<i.pos[1]){
                Insert(i,str,L,T)
                j=0
                break;
            }
        }
        if(j){
            str.ele.remove()
            str.ele.style.width=str.ele.style.height='50%'
            str.ele.style.position=''
            del(template,str)
            tree.children.push(str)
            tree.ele.append(str.ele)
            str.pos=[str.ele.getBoundingClientRect().top,str.ele.getBoundingClientRect().top+str.ele.offsetHeight,str.ele.getBoundingClientRect().left,str.ele.getBoundingClientRect().left+str.ele.offsetLeft+str.ele.offsetWidth]
        }
    }
}
```

重新点击标签之后对结构树中的对应元素进行删除：
```
function del(tree,ele){
    for(let i in tree){
        if(tree[i].ele.dataset.num==ele.ele.dataset.num){
            tree.splice(i,1)
        }else if(tree[i].children.length!=0){
            del(tree[i].children,ele)
        }
    }
}
```

dom结构改变之后，结构树中元素的绝对位置更新
```
function change(tree){
    for(let i of tree){
        i.pos=i.pos=[i.ele.getBoundingClientRect().top,i.ele.getBoundingClientRect().top+i.ele.offsetHeight,i.ele.getBoundingClientRect().left,i.ele.getBoundingClientRect().left+i.ele.offsetLeft+i.ele.offsetWidth]
        if(i.children.length){
            change(i.children)
        }
    }
}
```
![效果图](https://raw.githubusercontent.com/Briny131/dom-drag/master/2018-12-14_170235.png)
![git](https://raw.githubusercontent.com/Briny131/dom-drag/master/demo.gif)
