# useTabs

> 작지만 유용한 hooks!

1. MOCK데이터인 content를 만든다.

   ```jsx
   const content = [
     {
       tab: "Section 1",
       content: "I'm the content of the Section 1",
     },
     {
       tab: "Section 2",
       content: "I'm the content of the Section 2",
     },
   ];
   ```

2. content를 이용하여 <button>을 만들고 Section 1 / Section 2 를 클릭하면 해당하는 content를 보이도록 할 것이다.

   ```jsx
   const useTabs = (initialTab, allTabs) => {
     // Array.isArray(인자)  해당인자가 배열인지 알려주는것
     // 조건
     if (!allTabs || !Array.isArray(allTabs)) {
       return;
     }
     const [currentIndex, setCurrentIndex] = useState(initialTab);
     return {
       currentItem: allTabs[currentIndex],
       changeItem: setCurrentIndex,
     };
   };

   export default function App() {
     // const tabs = useTabs(0, content);
     // content[0] 과 같다

     // 실제로 현재 item의 content를 보여줘야한다.
     const { currentItem, changeItem } = useTabs(0, content);
     return (
       <div className="App">
         <h1>HI!</h1>
         {content.map((section, index) => (
           <button onClick={() => changeItem(index)}>{section.tab}</button>
         ))}
         <div>{currentItem.content}</div>
       </div>
     );
   }
   ```

3. 1. map()을 이용하여 <button> 을 만들어 주었다
4. useTab hook을 생성한다.
   - initialTab 값을 default로 가지는 state를 생성하고 그 state는 setCurrentIndex로 인해 변경된다.
5. const tabs = useTabs(0, content)로 initialTab값은 0 allTabs에는 content 가 들어간다.
6. if문을 이용하여 content가 없거나 배열이 아닐 경우 return 시킨다.
7. 버튼을 누르면 해당하는 content를 보일수 있도록 useTabs hook 은 currentItem: allTabs[currentIndex] 를 리턴 시킨다.
8. 그러면 <div>{currentItem.content}<div> initialTab값이 0 이기에 인덱스가 0인 "I'm the content of the Section 1" 가 보여진다.
9. button을 map()을 할때 map에서 자동으로 주어지는 Index를 사용해 onClick 이벤트가 발생하면 changeItem(index)를 작동시킨다.
   1. 그럼 useTab hook에서 return하는 changeItem: setCurrentIndex 가 작동해 currentIndex값을 전달받은 index값으로 바꾼다.
10. 그러면 다시 렌더링 되어서 클릭된 버튼의 인덱스 값의 content가 나오게 된다.
