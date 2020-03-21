# GoStack 10.0 || Desafio 05

* [1. Conceitos abordados](#1-conceitos-abordados)
* [2. Descrição do projeto](#2-descrição-do-projeto)
* [3. Iniciando o Projeto](#3-iniciando-o-projeto)
* [4. Hooks com Redux](#4-hooks-com-redux)

##  1. Conceitos abordados

1.  **useSelector (acessar uma informação do Redux).**
2.  **useDispatch (disparar uma action do Redux).**
3.  Eliminar connect, mapStateToProps, mapDispatchToProps.

___

## 2. Desrição do projeto:

Uso de useSelector e useDispatch.
1.  **useSelector (acessar uma informação do Redux).**
2.  **useDispatch (disparar uma action do Redux).**

Projeto Inicial: https://github.com/MaisDennis/GoStack10.0-Modulo-07

Projeto com React Hooks: https://github.com/MaisDennis/GoStack10.0-Modulo-07-ReactHooks

Um website 'Rocketshoes', que tem uma lista de produtos (tênis). O site permite adicionar os produtos ao 'Carrinho'. Na página do Carrinho,  podemos alterar a quantidade de cada produto, remover o produto e temos o retorno do valor subtotal e total de produtos em R$.

## 3. Iniciando o projeto

1.  Iniciar a biblioteca JSON Server:
```
json-server server.json -p 3333
```
2.  Iniciar o React
```
yarn start
```

## 4. Hooks com Redux

1. **Header/index.js**
    1. Trocar
        ```
        import { connect } from 'react-redux';
        ```
    1. por
        ```
        import { useSelector } from 'react-redux';
        ```
    1. Trocar
        ```
        function Header( { cartSize } ) {
        ```
    1. por
        ```
        export default function Header() {
          const cartSize = useSelector(state => state.cart.length);
        ```
    1. Deletar
        ```
        export default connect(state => ({
          cartSize: ,
        }))(Header);
        ```
    1. converter o restante dos componentes para useSelector e useDispatch.

1. **Home/index.js**
    1. Trocar
        ```
        import { connect } from 'react-redux';
        ```
    1. por
        ```
        import { connect, useSelector } from 'react-redux';
        ```
    1. criar:
        ```
        const amount = useSelector(state => state.cart.reduce((amount, product) => {
            amount[product.id] = product.amount;
            return amount;
          }, {}),)
        ```
    1. Deletar amount
        ```
        function Home( { amount, addToCartRequest }) {
        ```
    1. Deletar
        ```
        const mapStateToProps = state => ({
          amount: state.cart.reduce((amount, product) => {
            amount[product.id] = product.amount;
            return amount;
          }, {}),
        });
        ```
    1. Deletar mapStateToProps e deixar null no lugar.
        ```
        export default connect(mapStateToProps, mapDispatchToProps)(Home);
        ```
    1. Vide que Adicionar ao Carrinho continua a funcionar.
    2. Deletar
        ```
        const mapDispatchToProps = dispatch =>
          bindActionCreators(CartActions, dispatch);
        export default connect(null, mapDispatchToProps)(Home);
        ```
    1. add export default
        ```
        export default function Home( { addToCartRequest }) {
        ```
    1. Trocar
        ```
        import { connect, useSelector } from 'react-redux';
        ```
    1. por
        ```
        import { useDispatch, useSelector } from 'react-redux';
        ```
    1. Criar
        ```
        const dispatch = useDispatch();
        ```
    1. Remover addToCartRequest
        ```
        export default function Home( { addToCartRequest }) {
        ```
    1. Trocar o amount do state.cart.reduce para outra variavel = sumAmount
        ```
        export default function Home() {
          const [ products, setProducts ] = useState([]);
          const amount = useSelector(state => state.cart.reduce((sumAmount, product) => {
            sumAmount[product.id] = product.amount;
            return sumAmount;
          }, {}),)
        ```
    1.
      ```
      function handleAddProduct(id) {
          dispatch(CartActions.addToCartRequest(id));
        };
      ```

1. **Cart/index.js**
    1. Trocar
        ```
        import { connect } from 'react-redux';
        ```
    1. por
        ```
        import { useDispatch, useSelector } from 'react-redux';
        ```
    1. Deletar
        ```
        import { bindActionCreators } from 'redux';
        ```
    1. Deletar todas as props
        ```
        function Cart({ cart, total, removeFromCart, updateAmountRequest }) {
        ```
    1. Criar total
        ```
          const total = useSelector(state => formatPrice(
            state.cart.reduce((total, product) => { // reduce: 1 unico valor para todo array.
              return total + product.price * product.amount;
            }, 0),
          )
        ```
    1. Criar cart
        ```
        const cart = useSelector(state => state.cart.map(product => ({
            ...product,
            subtotal: formatPrice(product.price * product.amount),
          })),)
        ```
    1.
    ```
    const dispatch = useDispatch();
    ```
    1.
    ```
    dispatch(CartActions.updateAmountRequest(product.id, product.amount + 1));
    ```
    1. Deletar
    ```
    const mapStateToProps = state => ({
      cart: state.cart.map(product => ({
        ...product,
        subtotal: formatPrice(product.price * product.amount),
      })),
      total: formatPrice(
        state.cart.reduce((total, product) => { // reduce: 1 unico valor para todo array.
          return total + product.price * product.amount;
        }, 0),
      ),
    });
    const mapDispatchToProps = dispatch =>
      bindActionCreators(CartActions, dispatch);
    export default connect(mapStateToProps, mapDispatchToProps)(Cart);
    ```
    1.
    ```
    export default function Cart() {
    ```
    1. trocar total por totalSum
    ```
    state.cart.reduce((totalSum, product) => { // reduce: 1 unico valor para todo array.
          return totalSum + product.price * product.amount;
    ```
    1. Trocar
    ```
    <button type="button">
                      <MdDelete
                        size={20}
                        color="#7159c1"
                        onClick={() =>
                          removeFromCart(product.id)
                        }
                      />
                    </button>
    ```
    1. por
    ```
    dispatch(CartActions.removeFromCart(product.id))
    ```



