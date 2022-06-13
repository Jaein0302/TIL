# 0611

- **Generics**
    - **Box 클래스**
        - 클래스 이름 뒤에 **<E>**를 제네릭으로 적용하여 가상의 클래스를 사용한다는 의미
    
    ```java
    public class Box<E> {
            private E obj;
            public void setObj(E obj){
                this.obj = obj;
            }
    
            public E getObj(){
                return obj;
            }
        }
    ```
    
    - **BoxExam 클래스**
        - < >에 들어가는 타입만으로 인스턴스를 만들겠다는 의미
    
    ```java
    public class BoxExam {
            public static void main(String[] args) {
                Box<Object> box = new Box<>();
                box.setObj(new Object());
                Object obj = box.getObj();
    
                Box<String> box2 = new Box<>();
                box2.setObj("hello");
                String str = box2.getObj();
                System.out.println(str);
    
                Box<Integer> box3 = new Box<>();
                box3.setObj(1);
                int value = (int)box3.getObj();
                System.out.println(value);
            }
        }
    ```
    
- **Set**
    - Set 인터페이스의 add 메서드는 boolean형으로, 추가하고 나서 **중복**된 값이 있으면 **false를 return**한다
        
        **Set <String>** set1 = **new HashSet<>**();
        boolean flag1 = set1.add("kang");
        boolean flag2 = set1.add("kim");
        boolean flag3 = set1.add("kang");
        
    - 출력하기 위해서는 iteratror 객체를 이용한다
        
        **Iterator**<String> iter = set1.**iterator();**
        while(**iter.hasNext()**) {
            String str = **iter.next();**
            System.out.println(str);
        }
        
- **List**
    - 배열과 다른점은 **저장공간**이 필요에 따라 자유롭게 **바뀜**
    - 3가지 출력방법이 있고, 중복이 가능
