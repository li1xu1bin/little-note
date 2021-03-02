## 组合式 API 
setup 选项中没有 this
setup 接受 props 和 context

```js
import { toRefs } from 'vue'

export default {
  props: {
    title: String
  },
  setup(props) {
    const { title } = toRefs(props)
	  console.log(title.value)
    
  }
}
```

ref 函数使任何响应式变量在任何地方起作用
ref 接受参数并返回它包装在具有 value property 的对象中，然后可以使用该 property 访问或更改响应式变量的值：
ref 对我们的值创建了一个响应式引用

 ```js
setup() {
    const count = ref(1);

    const double = computed(() => count.value * 2);

    
    const increment = () => {
      count.value++;
    };

    watch(count, (newValue, oldValue) => {
      console.log('新的值是 ' + count.value)
    });

    onMounted(() => {
      console.info("onMounted");
    });
    return {
      count,
      increment,
      double
    };
  }

  ```  