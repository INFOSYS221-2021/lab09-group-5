# lab09-group-5
lab09-group-5 created by GitHub Classroom
brodie fogarty - bfog012
ricky xu - xxu281

GET is one of the HTTP request methods. What is a GET method? Give an example of how you can use GET with the Deck Of Cards API.
  
  A way for us to give the API some parameters. - Ways to pass parameters  
  use get parameter to define the number of Decks you want to use.

List all available features that you can use from Deck of Cards. For example, one of the APIs you can use is to shuffle cards.

  get a new deck shuffled 
  get a new deck unshuffled 
  get jokers added to deck 
  get a partial deck
  create card piles

What is the API for getting a brand new deck that also includes two jokers?

  https://deckofcardsapi.com/api/deck/new/?jokers_enabled=true 
  
  
How many cards are drawn from the deck? What are the cards? 

  2  king hearts  8 clubs 
  
How do you retrieve deck_id from the response? Write your code using JavaScript.

  async function createDeck() {
  // TODO 1: use the appropriate API to shuffle 
  // a new deck of card
  let response = await fetch('https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1');

  if (response.status === 200) {
    let data = await response.json();

    // TODO 2: get the id of the deck from data
    deckID = data.deck_id;
    return deckID;
  }
}

  
  
How do you print information about each card? Write your code using JavaScript.
  
  function getValueFromCard(cardInfo) {
  // TODO 4: find the value and suit of the card
  let cardValue = cardInfo.cards[0].value;
  let cardSuit = cardInfo.cards[0].suit;
    
Read and understand the JavaScript code snippet from previous step. Complete all six TODOs. Once you have completed the code, you will be able to run the game and see the cards on screen when you click the Hit Me! butto

// Initialise the values at the start of the game
// Do not change this part!
let thisDeck;
let numOfHits = 0;
let sum = 0;

// Getting different elements from HTML
// Do not change this part!
const hitMeBtn = document.getElementById('hitMeBtn');
const restartBtn = document.getElementById('restartBtn');
const hitMeSum = document.getElementById('hitMeSum');
const hitMeCards = document.getElementById('hitMeCards');
hitMeBtn.addEventListener('click', playGame);
restartBtn.addEventListener('click', restartGame);

// Complete the following TODOs for each function

// This function shuffles a new deck of card
// and returns the deck id
async function createDeck() {
  // TODO 1: use the appropriate API to shuffle 
  // a new deck of card
  let response = await fetch('https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1');

  if (response.status === 200) {
    let data = await response.json();

    // TODO 2: get the id of the deck from data
    deckID = data.deck_id;
    return deckID;
  }
}

// This function returns all available information
// about a card drawn from the deck created
async function getACard() {
  // TODO 3: use the appropriate API to draw one card 
  // from the deck created at the start of the game
  // Hint: use the variable thisDeck
  let response = await fetch('https://deckofcardsapi.com/api/deck/'+thisDeck+'/draw/?count=1');
  
  // retun card information
  // Do not change this
  if (response.status === 200) {
    let cardInfo = await response.json();
    return cardInfo;
  }
}

// This function returns the appropriate value
// based on cardInfo
function getValueFromCard(cardInfo) {
  // TODO 4: find the value and suit of the card
  let cardValue = cardInfo.cards[0].value;
  let cardSuit = cardInfo.cards[0].suit;
  
  // TODO 5: update the cardValue appropriately
  // if card is Jack, then set the cardValue to 11, etc
  if (cardValue == 'JACK') {
    cardValue = 11; 
  } 
  if (cardValue == 'QUEEN') {
    cardValue = 12; 
  }
  if (cardValue == 'KING') {
    cardValue = 13; 
  } 
  if (cardValue == 'ACE') {
    cardValue = 1; 
  } 
  
  // if a card is red, then the value will be negative
  // Do not change this.
  let posOrNeg = 1;
  if (cardSuit == 'DIAMONDS' || cardSuit == 'HEARTS') {
    posOrNeg = -1;
  }
  
  return cardValue * posOrNeg;
}

// This function gets the image from cardInfo
// and updates the HTML
function updateCardImg(cardInfo) {
    // TODO 6: find the image of the card
  let imgUrl = cardInfo.cards[0].image;
  
  // update HTML. Do not change this.
  hitMeCards.innerHTML += "<img src='" + imgUrl + "' width='100' />";
}

// This function controls the flow of the game
// Do not change this function!
async function playGame() {
  // get a new deck of card if we just start the game
  if (numOfHits === 0) {
    thisDeck = await createDeck();
  }
  
  // update the number of hits and button
  numOfHits = numOfHits + 1;
  restartBtn.disabled = false;
  
  // get card information and image, and
  // update the sum of cards
  cardInfo = await getACard();
  updateCardImg(cardInfo);
  sum += getValueFromCard(cardInfo);
  
  // check if we have reached the end of the game, and
  // update HTML appropriately
  if (numOfHits > 4) {
    hitMeSum.innerHTML = "End of Game! Total value of your cards is " + sum;
    hitMeBtn.disabled = true;
  } else {
    hitMeSum.innerHTML = "Current total value of your cards is " + sum;
  }
}

// This function resets the game
// Do not change this function!
function restartGame() {
  numOfHits = 0;
  sum = 0;
  hitMeBtn.disabled = false;
  restartBtn.disabled = true;
  hitMeSum.innerHTML = "Click Hit Me! to start playing!"
  hitMeCards.innerHTML = "";
}
