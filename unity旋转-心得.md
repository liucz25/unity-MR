# 旋转角度有两种方法

## 欧拉角

   常规人类思路，x,y,z, 三个方向的360度为标准的度数，

## 四元数

  计算机视角，据说能防止 万向节 死锁。

## 万向节 死锁

当X轴旋转90 度后，y和z 方向上的旋转变得相同，如果物体是架飞机，X是俯仰，y是拐弯，z是轴向旋转。当机头朝向下方时，y 也变成轴向旋转，不能拐弯了

## 四元数是否能解决，不确定，转换欧拉角的顺序能部分解决，死锁方向变了。

# unity 的 7种旋转方法

## 1 rotation

```cs
transform.rotation = Quaternion.Euler(30,30,30) ;
```

Quaternion.Euler 方法可以把 欧拉角 转换成 四元数

```cs
float angle;
transform.rotation=Quaternion.Euler(0,0,angle++);
```

这样也行，支持变量，能自加

## ２ localRotation

```cs
float angle;
transform.localRotation=Quaternion.Euler(0,0,angle++);
```

localRotation　相对父对象

## 3 Rotate 方法

```cs
transform.Rotate(new Vector(0,1,0),1f,Space.World);
```

Vector 旋转围绕的轴，x轴：（1,0,0）Y轴：（0,1,0），z轴（0,0,1）

1f 每帧旋转角度

Space.World 参考系坐标

#### 变体

```cs
transform.Rotate(new Vector(0,1f,0));
```

0,1f,0 代表每帧旋转的数值，越小越慢

## 4 eulerAngles

```cs
transform.eulerAngles=new Vector(0,1f,0);
```

#### 5 变体localEulerAngles

```cs
transform.localEulerAngles=new Vector(0,1f,0);
```

eulerAngles 和 rotate 区别，eulerAngles是欧拉角，rotate是四元数

### 6 slerp  插值

```cs
var target=Quaternion.Euler(new Vector(0,180,0));
transform.rotate=Quaternion.Slerp(transform.rotate,target,Time.deltaTime);
```

#### 变体

```cs
var target=Quaternion.Euler(new Vector(0,180,0));
var t=1/(transform.rotate.eulerAngles-target.eularAngles).magnitude;
transform.rotate=Quaternion.Slerp(transform.rotate,target,t);
```

### 7 RoutateAround 方法

```cs
public void RotateAround(Vector3 point,Vector3 axis,float angle);
##point：围绕的中心  axis：围绕的轴 angle：旋转角度
```

### 8 刚体 angularVelocity

以弧度每秒 为单位测量的刚体角速度矢量，

在大多数情况下，你不应该直接修改它，因为这会导致不切实际的行为。
