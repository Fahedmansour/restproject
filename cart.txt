let label =document.getElementById("label")  ;
let shoppingCard = document.getElementById("shopping-card"); 
let basket= JSON.parse(localStorage.getItem("data")) ||[];

let calculation=()=>{
  let cartIcon=document.getElementById("cardAmount");
  cartIcon.innerHTML=basket.map((x)=>x.item).reduce((x,y)=>x+y,0);
 
}
calculation();
let generateCartItem = ()=>{
    if (basket.length !== 0){
        return  (shoppingCard.innerHTML = basket.map((x)=>{
    let {id,item}=x ;
    
                                    return  (`
                                    <div class="cart-items"  >
                                        
                                      <div class="cart-img">
                                    <img class="img-cart" src="${products[id].picture}"/>
                                    </div>
                                    <div class="cart-text"  >
                                    <h4 id="item_1">${products[id].name}</h4>
                                    <div class ="cart-product-price">${products[id].price} Dt</div>
                                    <div>
                                    <i class="fa-regular fa-heart"></i>                             
                                     <button class="add" onclick="increment(${id})">+</button>
                                     <span id=${id}>${item}</span>
                                     <button class="delate" onclick="decrement(${id})">-</button>
                                      </div>
                                    </div>
                                    
                                      <div ><div class="total-price">
                                        <h4>Total price : </h4>
                                        <h3>${item*products[id].price}DT</h3>
                                     </div>
                                            <i onclick="removeItem(${id})" class="bi-x-lg"></i>
                                        </div>
                                    </div>
                                   
                                  </div>
                                    `);

  }).join(""));
}else{
        shoppingCard.innerHTML=``;
        label.innerHTML=`
        <h2> Cart in empty</h2>
   <a href="index1.html">
    <button class="homebtn">Back to home</button>
   </a>
        `
        
    }
}
generateCartItem();
let increment=(id)=>{
    let selectedItem=id;
    
    let search=basket.find((x)=>x.id===selectedItem);
    if(search===undefined){
    basket.push({
      id :selectedItem,
      item:1,
    });
  }else{
    search.item+=1;
  }
  
  update(selectedItem);;
  localStorage.setItem("data",JSON.stringify(basket)  )
    console.log(basket );
    console.log(selectedItem );
  };



  let decrement=(id)=>{
    let selectedItem=id;
    let search=basket.find((x)=>x.id===selectedItem);
    if(search.item===undefined)return;
    else if(search.item===0)return;
    else{
    search.item-=1;
  }
  update(selectedItem);
  basket= basket.filter((x)=>x.item !==0);
  generateCartItem();
  localStorage.setItem("data",JSON.stringify(basket)  )
  console.log(basket );
  };

  let update=(id)=>{
    console.log(id)
   let search=basket.find((x)=>x.id=== id);
    //console.log(search.item);
    document.getElementById(id).innerHTML=search.item;
   calculation();
   totelAmound();
  };
let removeItem=(id)=>{
    let selectedItem=id;
    basket=basket.filter((x)=>x.id !==selectedItem);
    generateCartItem();
    totelAmound();
    localStorage.setItem("data",JSON.stringify(basket));

}

let clearCart=(id)=>{
    basket=[]
    generateCartItem();
    calculation();
    localStorage.setItem("data",JSON.stringify(basket));
}

let totelAmound=()=>{
    if(basket.length !==0){
        let amound=basket.map((x)=>{
            let {item,id}=x;
           
            return item*products[id].price;
        }).reduce((x,y)=>x+y,0);
        label.innerHTML=`
        <h2>Total Bill : ${amound} DT</h2>
   <button  class="checkout">Checkout</button>
   <button onclick="clearCart()" class="removeAll">Clear charset</button>
        `

    }else return;
};
totelAmound();