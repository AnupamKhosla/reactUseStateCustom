# reactUseStateCustom
Simplified reverse engineering of useState hook in react

 ### Demo: https://jsfiddle.net/fx4Ltdzm/
 
 #### Complete hooks reverse engineering https://medium.com/swlh/learn-by-implementing-reacts-usestate-and-useeffect-a-simplified-overview-ea8126705a88
 
 #### My Code:
 
```
const manualRerender = () => {
  hookCount = -1; //recount total hook initializations on every render
  root.render(<App />);
}

let hookCount = -1;
const states = {}; //hooks array/object

const TheuseState = (value) => {
  const id = ++hookCount;  //every hook call increases the hook count  
  const setValue = (newValue) => {
    states[id][0] = newValue;
    console.log(states);
    manualRerender();
  }
  if (states[id] == undefined) { //first render
    states[id] = [value, setValue];
  }
  else { //later renders don't get _value from TheuseState(_value)
    states[id] = [states[id][0], setValue];
  }
  const state = states[id];
  return state;
}

function App(props) {
  const [count, setCount] = TheuseState(1);
  //console.log([count, setCount]);
  const [msg, setMsg] = TheuseState("Bye");
  //console.log([msg, setMsg]);
  console.log(states);
  return (
    <div className="App">
      <h2>Anupam khosla Counter</h2>
      <button onClick={()=> setCount(count+1)}>+++</button>
      <p>{count}</p>
      <button onClick={()=> setCount(count-1)}>---</button>
      <br/>
      <br/>
      <button onClick={()=> setMsg(msg+msg)}>{msg}</button>
    </div>
  );
}

const root = ReactDOM.createRoot(document.getElementById("app"));
root.render(<App />);
```
