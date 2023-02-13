const productsList = document.getElementById("row");
let basket= JSON.parse(localStorage.getItem("data")) ||[]; 
let renderProducts = (()=>{
return  productsList.innerHTML = products.map((x)=>{
  let {id,name,picture,price}=x ;
  let search=basket.find((x)=>x.id===id)||[];
                                  return  (`
                                  <div class="items"   id="product-id-${id}">
                                    <div class="pic-product">
                                  <img class="img" src="${picture}"/>
                                  <h4 id="item_1">${name}</h4>
                                  <div class ="product-price">${price} <span>Dt/Kg</span></div>
                                  <i class="fa-regular fa-heart"></i>
                                  <div>
                                   <button class="add" onclick="increment(${id})">+</button>
                                   <span id=${id}> ${search.item===undefined? 0:search.item} </span>
                                   <button class="delate" onclick="decrement(${id})">-</button>
                                    </div>
                                  </div>
                                </div>
                                  `);

}).join("")});
renderProducts(); 
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

localStorage.setItem("data",JSON.stringify(basket)  )
console.log(basket );
};

let update=(id)=>{
  console.log(id)
 let search=basket.find((x)=>x.id=== id);
  //console.log(search.item);
  document.getElementById(id).innerHTML=search.item;
  calculation();
};





let calculation=()=>{
let cartIcon=document.getElementById("cardAmount");
cartIcon.innerHTML=basket.map((x)=>x.item).reduce((x,y)=>x+y,0);

}
calculation();