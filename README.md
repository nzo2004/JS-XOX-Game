JS kodları ne işe yarar = 

//X seçeriz
const xClass='cross'

//O seçeriz
const oClass='circle'
//Bütün kazanma kombinasyonları belirleriz
const combinations=[
    [0,1,2],
    [3,4,5],
    [6,7,8],
    [0,3,6],
    [1,4,7],
    [2,5,8],
    [0,4,8],
    [2,4,6]
]

//duvar kısmını seçeriz
const board=document.getElementById('board');

//X ve O koyacağımız yerleri seçeriz
const cells=document.querySelectorAll('.cell');

//sonuç id seçeriz
const result=document.getElementById('result');

//sonuç yazısı id seçeriz
const resultText=document.querySelector('.result-text');

// reset id seçeriz
const restartBtn=document.getElementById('reset');

let turn; // False X , True O

//Tam tersi yapar 
const swapTurn = () =>{turn=!turn};
//seçtiğimiz şeyi eklemek için kullanılır
const placeMark=(cell,currentClass)=>{cell.classList.add(currentClass)}

const placeHover = () => {
    //Varsa silinsinler der
    board.classList.remove(xClass)
    board.classList.remove(oClass)
    //turn ture ise O class 
    //false ise X class eklensin
    if(turn) board.classList.add(oClass)
    else board.classList.add(xClass)
}

//Sonuc belirleriz
const endGame=(draw)=>{
    //Eğer beraberlik için
    if(draw) 
    {
        resultText.innerText ='Beraberlik!'
    }
    //Eğer kazanan olursa
    else 
    {
        resultText.innerText= `${turn ? 'O' : 'X'} Kazandi!`
    }

    //ekrana yazı olarak ekleriz
    result.classList.add('show')
}

const isDraw=()=>{
    //Bütün yeri dizi halinde alırız
    return [...cells].every(cell=>{
        //contains ile X ve O class var mı diye sorarız
        return cell.classList.contains(xClass) || cell.classList.contains(oClass)
    })
}
const checkWin=(currentClass)=>{
    //combinations.some dizimiz ile eşleşen var mı diye sorarız
    return combinations.some(combination=>{
        return combination.every(index=>{
            return cells[index].classList.contains(currentClass)
        })
    })
}

const handleClick=(e)=> {
    //neye tıklandığı söyleriz
    const cell=e.target
    //O mu X mi
    const currentClass=turn ? oClass : xClass
    placeMark(cell,currentClass)
    //Kazanan var mı
    if(checkWin(currentClass)){
        //varsa bitir
        endGame(false)
    //yoksa devam et
    }else if(isDraw()){
        endGame(true)
    }else{
        swapTurn()
        placeHover()
    }
}
//Yeniden başlatırken
const resetGame=()=>{
    cells.forEach(cell=>{
        //Bütün X ve O ları sil deriz
        cell.classList.remove(xClass)
        cell.classList.remove(oClass)
        //Bütün clickleri sil
        cell.removeEventListener('click',handleClick)
        //Baştan başla
        cell.addEventListener('click',handleClick, {once:true})
    })
}


//Başlatmak için
const startGame=() => {
    turn=false
    resetGame()
    placeHover()
    result.classList.remove('show')
}

startGame()
restartBtn.addEventListener('click',startGame)
