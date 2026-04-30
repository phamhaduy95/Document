using Single file declaration approach which put CSS, JS and HTML template in one place instead of separated file

Props declaration
- controllable value 
- on event action


type-based props declaration
This is called "type-based declaration". The compiler will try to do its best to infer the equivalent runtime options based on the type argument. In this case, our second example compiles into the exact same runtime options as the first example.


Run-time declaration

since Vue allow extra props to be passed through, we don't need to explicitly extends component default attributes  

always let parent component to mutate the state object. reason https://vuejs.org/guide/components/props.html#mutating-object-array-props
defining slot for customizing inner components

task management in turborepo

- signal cache miss for turbo to rerun task

we can