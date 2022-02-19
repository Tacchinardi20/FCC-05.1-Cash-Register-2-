function checkCashRegister(price, cash, cid) {
  // Set up variables
  var change = cash-price;
  var changeList = [["PENNY", 0], ["NICKEL", 0], ["DIME", 0], ["QUARTER", 0], ["ONE", 0], ["FIVE", 0], ["TEN", 0], ["TWENTY", 0], ["ONE HUNDRED", 0]];
  var totalInRegister = 0;
  var denomination;
  //calculating total in register
  for(let i in cid){
    totalInRegister+=cid[i][1];
  }
  //Check if register has enough money
  if(totalInRegister<change){
    return {status: "INSUFFICIENT_FUNDS", change: []}
  }
  //check if register has the exact amount of money
  else if(totalInRegister==change){
    return {status: "CLOSED", change: cid}
  }
  //Selecting next denomination
  for(let i = 8; i>=0;i--){
    switch(i){
      case 8: denomination = 100; break;
      case 7: denomination = 20; break;
      case 6: denomination = 10; break;
      case 5: denomination = 5; break;
      case 4: denomination = 1; break;
      case 3: denomination = 0.25; break;
      case 2: denomination = 0.1; break;
      case 1: denomination = 0.05; break;
      case 0: denomination = 0.01; break;
    }
    //giving the denomination if appropriate
    while(cid[i][1]>=denomination && change>=denomination){
      cid[i][1] = Math.round((cid[i][1]*100)-(denomination*100))/100;
      changeList[i][1] = Math.round((changeList[i][1]*100)+(denomination*100))/100;
      change = Math.round((change*100)-(denomination*100))/100;
    }
    if(changeList[i][1]==0){
      changeList.splice(i,1);
    }
  }
  //If not all change was given
  if(change>0){
    return {status: "INSUFFICIENT_FUNDS", change: []}
  }
  return {status: "OPEN", change: changeList.reverse()};
}
var answer = checkCashRegister(19.5, 20, [["PENNY", 0.01], ["NICKEL", 0], ["DIME", 0], ["QUARTER", 0], ["ONE", 1], ["FIVE", 0], ["TEN", 0], ["TWENTY", 0], ["ONE HUNDRED", 0]]);
console.log(answer["status"] + "   ///   " + answer["change"]);