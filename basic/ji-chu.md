# 基础

## 基本

| 类型 | 长度 | 备注 |
| :--- | :--- | :--- |
| bool | 1 |  |
| byte | 1 | 相当于uint8 |
| rune | 4 | int32,存储Unicode Code Point |
| int/uint |  |  |
| int8/uint8 | 1 | -128~ 127  ; 0 ~ 255 |
| int16/uint16 | 2 | -32768 ~ 32767; 0 ~ 65535 |
| int32/uint32 | 4 | -21亿 ~ 21亿, 0 ~ 42亿 |
| int64/uint64 | 8 |  |
| float32 | 4 | 精确到 7 个⼩数位 |
| float64 | 8 | 精确到 15个⼩数位 |
| complex64 | 8 |  |
| complex128 | 16 |  |
| uintptr |  | 足够保存32位或者64位整数 |
| array |  | 值类型 |
| struct |  | 值类型 |
| string |  | 值类型 |
| slice |  | 引用类型 |
| map |  | 引用类型 |
| channel |  | 引用类型 |
| interface |  | 接口类型 |
| function |  | 函数类型 |

## Array

```text
func main() {
	var temp [10]int
	fmt.Println("temp:", temp)

	var twoD [2][3]int
	for i := 0; i < 2; i++ {
		for j := 0; j < 3; j++ {
			twoD[i][j] = i + j
		}
	}
	fmt.Println("twoD:", twoD)
}
```

## Slice

```text
func main() {
	s := make([]string, 3)
	fmt.Println("default value:", s)

	s[0] = "a"
	s[1] = "b"
	s[2] = "c"
	fmt.Println("get", s)
	fmt.Println("length:", len(s))
	s = append(s, "d")
	s = append(s, "e")
	fmt.Println("after append", s)
	fmt.Println("after append length", len(s))

	c := make([]string, len(s))
	copy(c, s)
	fmt.Println("s copy from c", c)
	a1 := s[:5]
	a2 := s[2:5]
	a3 := s[2:]
	fmt.Println(a1, a2, a3)

	s[1] = "z"
	fmt.Println("update s[1] set value is z", s)
	fmt.Println(c)

}
```

## Map

```text
func main() {
	m := make(map[string]int, 10)
	fmt.Println("after new a map:", m)
	// add
	m["key1"] = 1
	m["key2"] = 2
	m["key3"] = 3
	fmt.Println("after add element to map:", m)
	// delete
	delete(m, "key2")
	fmt.Println("after delete element from map:", m)
	// update
	m["key1"] = 10
	fmt.Println("after update element from map:", m)
	// get
	fmt.Println("after get value from map: ", m["key1"])

	for key, value := range m {
		// 由于map与slice的element是共享的，
		//所以在循环中修改element会体现到原有的容器中
		fmt.Println("get a key from map： ", key)
		fmt.Println("get a value from map: ", value)
	}

	// use map implement set map[type] bool
	SetMap := make(map[int]bool, 1)
	SetMap[1] = true
	SetMap[2] = true
	// select a element
	checkValueIsExisting(SetMap, 2)

	// delete
	delete(SetMap, 2)
	fmt.Printf("after delete %d form map\n", 2)
	// select a element
	checkValueIsExisting(SetMap, 2)

}

func checkValueIsExisting(m map[int]bool, n int) {
	if m[n] {
		fmt.Printf("element value = %d is existing\n", n)
	} else {
		fmt.Printf("element value = %d is not existing\n", n)
	}
}
```

## Function

```text
func main() {
	result := plus(3, 4)
	fmt.Println("plus=", result)

	x, y := multipleValuesFunction(1, 2)
	fmt.Println("multipleValuesFunction get value x：", x)
	fmt.Println("multipleValuesFunction get value y：", y)

	value := variadicFunction(1, 2, 3, 4, 5)
	fmt.Println("variadicFunction get value :", value)

	closure := closureFunction()
	fmt.Println("closure first count : ", closure())
	fmt.Println("closure second count : ", closure())
	newClosure := closureFunction()
	fmt.Println("new closure fist count:", newClosure())
}

func plus(a int, b int) int {
	return a + b
}

func multipleValuesFunction(x int, y int) (int, int) {
	return x, y
}

func variadicFunction(nums ...int) int {
	var result int
	for i := 0; i < len(nums); i++ {
		result = result + nums[i]
	}
	return result
}

func closureFunction() func() int {
	i := 0
	return func() int {
		i++
		return i
	}
}
```

## Method

```text
type rect struct {
	width, height int
}

func (r *rect) area() int {
	return r.width * r.height
}

func (r *rect) perim() int {
	return 2*r.width + 2*r.height
}

func main() {
	r := rect{width: 3, height: 4}

	fmt.Println("area:", r.area())
	fmt.Println("perim", r.perim())
	rcopy := &r
	rcopy.width = 12
	fmt.Println("after change area:", r.area())
}
```

## 类型转换

是不支持隐式转换的，必须显示进行转换type.\(expression\)

```text
func main() {
 a := 0x1234
 b := 1234.56
 c := 256
 fmt.Printf("%x\n", uint8(a)) // 截短⻓度
 fmt.Printf("%d\n", int(b)) // 截断⼩数
 fmt.Printf("%f\n", float64(c))
}

//34
//1234
//256.000000
```

## 常量

常量必须是编译期能确定的 Number \(char/integer/float/complex\)、String 和 bool 类型。可以 在函数内定义局部常量

```text
const x uint32 = 123
const y = "Hello"
const a, b, c = "meat", 2, "veg" ! ! // 同样⽀持⼀次定义多个常量。
const (
 z = false
 a = 123
)
```

## 枚举iota

iota从0开始增加，按照行进行递增

```text
const (
 Sunday = iota // 0
 Monday! ! // 1
 Tuesday! ! // 2
 Wednesday! ! // 3
 Thursday! ! // 4
 Friday! ! // 5
 Saturday! ! // 6
)
```

如果想要恢复递增，则必须再次使用iota.

```text
func main() {
 const (
 a = iota // 0
 b! ! // 1
 c! ! // 2
 d = "ha" // 独⽴值
 e! ! // 没有使⽤ iota 恢复，所以和 d 值相同
 f = 100
 g! ! // 和 f 值相同
 h = iota // 7, 恢复 iota couter，继续按⾏递增。
 i! ! // 8
 )
 println(a, b, c, d, e, f, g, h, i)
}

// 输出 0 1 2 ha ha 100 100 7 

```













·\`\`\`

