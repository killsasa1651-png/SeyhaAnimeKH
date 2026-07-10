<!DOCTYPE html>
<html>
<head>

<title>Seyha Anime KH</title>

<meta name="viewport" content="width=device-width, initial-scale=1">

<style>

body{
    margin:0;
    background:#080012;
    color:white;
    font-family:Arial;
}


/* FIRST PAGE */

#home{
    height:100vh;
    display:flex;
    flex-direction:column;
    justify-content:center;
    align-items:center;
    text-align:center;
}


.logo{
    font-size:35px;
    color:#b55cff;
}


.searchBox{
    width:85%;
    max-width:500px;
    padding:15px;
    border-radius:30px;
    border:none;
    font-size:18px;
    margin-top:25px;
}




#results{

    padding:20px;
    display:none;

}



.grid{

display:grid;
grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
gap:15px;

}



.card{

background:#21003d;
border-radius:15px;
overflow:hidden;
text-align:center;
padding-bottom:10px;

}



.card img{

width:100%;
height:220px;
object-fit:cover;

}



button{

background:#a855f7;
color:white;
border:none;
padding:8px 15px;
border-radius:20px;

}




#detail{

display:none;
position:fixed;
background:#080012;
top:0;
left:0;
right:0;
bottom:0;
padding:20px;
overflow:auto;

}



#detail img{

width:250px;
border-radius:20px;

}


</style>

</head>


<body>



<!-- FIRST PAGE -->

<div id="home">

<div class="logo">

🎬 Seyha Anime KH

</div>


<p>
Anime & Donghua Catalog
</p>


<input 
class="searchBox"
id="search"
placeholder="🔍 Search Anime...">


</div>






<!-- RESULTS -->

<div id="results">

<h2>🔥 Anime Results</h2>

<div id="list" class="grid"></div>

</div>







<!-- DETAIL -->

<div id="detail">

<button onclick="closeDetail()">
⬅ Back
</button>


<div id="detailBox"></div>


</div>








<script>


let anime=[];

let list=document.getElementById("list");

let search=document.getElementById("search");

let results=document.getElementById("results");

let home=document.getElementById("home");

let detail=document.getElementById("detail");

let detailBox=document.getElementById("detailBox");


let favorites=
JSON.parse(localStorage.getItem("fav")) || [];





fetch("https://api.jikan.moe/v4/top/anime")

.then(r=>r.json())

.then(d=>{

anime=d.data;

});







function show(data){


list.innerHTML="";


data.forEach(a=>{


list.innerHTML+=`

<div class="card">


<img onclick="openDetail(${a.mal_id})"

src="${a.images.jpg.image_url}">


<h3>${a.title}</h3>


⭐ ${a.score || "N/A"}


<br>


<button onclick="favorite(${a.mal_id})">

${favorites.includes(a.mal_id)?"❤️":"🤍"}

</button>


</div>


`;

});


}







search.oninput=function(){


let text=this.value.toLowerCase();



if(text.length>0){


home.style.display="none";

results.style.display="block";



let found=anime.filter(a=>

a.title.toLowerCase().includes(text)

);



show(found);


}


};








function favorite(id){


if(favorites.includes(id)){


favorites=favorites.filter(x=>x!=id);


}else{


favorites.push(id);


}


localStorage.setItem(

"fav",

JSON.stringify(favorites)

);


}








function openDetail(id){


let a=anime.find(x=>x.mal_id==id);


detail.style.display="block";


detailBox.innerHTML=`

<h1>${a.title}</h1>


<img src="${a.images.jpg.large_image_url}">


<h3>⭐ Rating: ${a.score}</h3>


<h3>🎬 Episodes: ${a.episodes}</h3>


<p>${a.synopsis || "No Description"}</p>


<a href="${a.url}" target="_blank">

<button>
▶️ Watch Official
</button>

</a>


`;

}




function closeDetail(){

detail.style.display="none";

}


</script>


</body>
</html>

