# Vue-custom-select

### $props:
| Name 					| Type 					| Default 		| Description |
| ------------------ | :----------------:	| :----------: | ----------- |
| value 					| `null` 				| **any** 		| *Текущее выбранное значение* |
| required 				| `null` 				| **any** 		| *Добавление `*` для label* |
| type 					| `String` 				| **text** 		| *Тип инпута* |
| label 					| `String` 				| **''** 		| *Установка label* |
| target 				| `String` 				| **''** 		| *Устоновка класса, на стилевой контейнер инпута* |
| placeholder 			| `String` 				| **''** 		| *Устоновка placeholder* |
| height 				| `[String, Number]` | **48** 		| *Установка высоты инпута* |
| isFill 				| `Boolean` 			| **false** 	| *Включить автозаполнение (применяется для type="password")* |
| isLoadFocus 			| `Boolean` 			| **false** 	| *Включить автофокус при загрузке компонента* |

### $events:
```js
	$emit('blur', value)
	$emit('input', value)
	$emit('focus', value)
	$emit('enter', value)
```