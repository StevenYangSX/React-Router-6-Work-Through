# React Router 6 Work Through

## 1. Official Tutorial

1. ######  Create A React App, modify the `index.js`file. We need to wrap the `<App/>`component with `<BrowserRouter>...</BrowserRouter>`

   ```tsx
   import ReactDOM from "react-dom/client";
   import { BrowserRouter } from "react-router-dom";
   import App from "./App";
   
   const root = ReactDOM.createRoot(
     document.getElementById("root")
   );
   root.render(
     <BrowserRouter>
       <App />
     </BrowserRouter>
   );
   ```

2. We add some basic `Links` in `App.js` file. These `<Link to="/somewhere">...</Link>` will change our URL.

   ```tsx
   import { Link } from "react-router-dom";
   
   export default function App() {
     return (
       <div>
         <h1>Bookkeeper</h1>
         <nav
           style={{
             borderBottom: "solid 1px",
             paddingBottom: "1rem",
           }}
         >
           <Link to="/invoices">Invoices</Link> |{" "}
           <Link to="/expenses">Expenses</Link>
         </nav>
       </div>
     );
   }
   ```

3. Add Some Routes : Here, "Routes" means some components to be rendered when URL changes. Under `src/routes/..` ,we created `expenses.jsx` , `invoices.jsx` files. 

```tsx
// expenses.jsx 	
export default function Expenses() {
  return (
    <main style={{ padding: "1rem 0" }}>
      <h2>Expenses</h2>
    </main>
  );
}

// invices.jsx

export default function Invoices() {
  return (
    <main style={{ padding: "1rem 0" }}>
      <h2>Invoices</h2>
    </main>
  );
}
```

4. Then we could "teach" React Router how to render at different URLs.
   1. `localhost:3000/` renders `<App/>`
   2. `localhost:3000/expense` renders `<Expense/>`
   3. `localhost:3000/invoices` renders `<Invoices/>`

```jsx
//.....
root.render(
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<App />} />
      <Route path="expenses" element={<Expenses />} />
      <Route path="invoices" element={<Invoices />} />
    </Routes>
  </BrowserRouter>
);
//.....
```

5. Nested Routes : In the code above, as long as you click "expense" or "invoices" links, the content in `<App/>`disappears. If we **want to use some automatic, persistent layout**, we need to do the following:
   1. Nest the routes inside of the App route
   2. Render an **Outlet** : 
      1. I personally think **Outlet** is just a **plaveholder** for chidlren components in a route.
      2. In another word, If we want **nested** componets, we need to use **Outlet** in the parent component.

```jsx
//.....
root.render(
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<App />}>
        <Route path="expenses" element={<Expenses />} />
        <Route path="invoices" element={<Invoices />} />
      </Route>
    </Routes>
  </BrowserRouter>
);
//.....
```

```jsx
import { Outlet, Link } from "react-router-dom";

export default function App() {
  return (
    <div>
      <h1>Bookkeeper</h1>
      <nav
        style={{
          borderBottom: "solid 1px",
          paddingBottom: "1rem",
        }}
      >
        <Link to="/invoices">Invoices</Link> |{" "}
        <Link to="/expenses">Expenses</Link>
      </nav>
      <Outlet />
    </div>
  );
}
```



6. Adding a "No Match" Route : we use `paht="*"` to match only when **NO** other routes do.
7. Reading URL Params : 
   1. We use `useParams` to get the URL params.

```jsx
import { useParams } from "react-router-dom";

export default function Invoice() {
  let params = useParams();
  return <h2>Invoice: {params.invoiceId}</h2>;
}
```

8. Index Routes: Official document has 4 explanation for this concept. I like this one the most! 

   **Index routes are the default child route for a parent route.**

   **解释：当进入某一级路由之后，如果用户没有点击Link进入其下一及路由，也就是说，当前URL匹配到了一个父级路由，但是没匹配到任何子路由，这时候会渲染 Index Route的element的内容**

```jsx
//.....
<Route path="invoices" element={<Invoices />}>
      <Route
        index
        element={
          <main style={{ padding: "1rem" }}>
            <p>Select an invoice</p>
          </main>
        }
      />
      <Route path=":invoiceId" element={<Invoice />} />
    </Route>
//......
```

9. Active Links   :  **`<NavLink style={({ isActive })=>{return ...}}/>`**

   **This is very useful. NavLink has style prop and style can be a function with para called  isActive...**

   **<NavLink className={({ isActive }) => isActive ? "red" : "blue"} />**

10. Search Params:   (query parmas)   **`localhost:3000/shoes?barnd=nike&sort=asc`**

    we use  **`useSearchParams`** to setup query parameters in the URL.

    ```jsx
    //.......
    let [searchParams, setSearchParams] = useSearchParams();
    //......
    	 <input
              value={searchParams.get("filter") || ""}
              onChange={(event) => {
                let filter = event.target.value;
                if (filter) {
                  setSearchParams({ filter });
                } else {
                  setSearchParams({});
                }
              }}
            />
    //......
    ```

    

