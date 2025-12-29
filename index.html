<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Single-file Chess</title>
<style>
  :root{
    --light:#f0d9b5;
    --dark:#b58863;
    --accent:#f39c12;
    --highlight:#7fc7ff66;
    --board-size: min(90vmin, 720px);
  }
  body{
    display:flex;
    align-items:center;
    justify-content:center;
    min-height:100vh;
    margin:0;
    background: linear-gradient(180deg,#0f1724,#071023);
    font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    color:#e6eef8;
    padding:20px;
  }
  .app{
    width:calc(var(--board-size) + 320px);
    max-width:calc(100% - 40px);
    display:flex;
    gap:18px;
    align-items:flex-start;
  }
  .board-wrap{
    width:var(--board-size);
    height:var(--board-size);
    display:grid;
    grid-template-columns:repeat(8,1fr);
    grid-template-rows:repeat(8,1fr);
    border-radius:12px;
    box-shadow: 0 10px 30px rgba(2,6,23,0.6);
    overflow:hidden;
  }

  .square{
    width:100%;
    height:100%;
    display:flex;
    align-items:center;
    justify-content:center;
    font-size:calc(var(--board-size) / 14);
    user-select:none;
    cursor:pointer;
    position:relative;
  }
  .square.light{ background:var(--light); color:#111; }
  .square.dark{ background:var(--dark); color:#111; }

  .square.selected{ outline:4px solid var(--accent); box-sizing:border-box; z-index:2;}
  .square.legal::after{
    content:"";
    position:absolute;
    width:28%;
    height:28%;
    border-radius:50%;
    background: rgba(0,0,0,0.18);
    box-shadow: inset 0 0 0 6px rgba(255,255,255,0.06);
  }
  .square.capture::after{
    content:"";
    position:absolute;
    width:80%;
    height:80%;
    border-radius:12%;
    background:var(--highlight);
    opacity:0.9;
  }
  .square.in-check{ box-shadow: inset 0 0 0 5px rgba(255,0,0,0.35);}

  .piece{
    pointer-events:none;
    font-size:1.1em;
    transform: translateY(2%);
  }

  /* Right column UI */
  .panel{
    width:300px;
    max-height:var(--board-size);
    background: linear-gradient(180deg,#071425 0%, #051824 100%);
    border-radius:12px;
    padding:16px;
    box-shadow: 0 10px 30px rgba(3,8,20,0.6);
    display:flex;
    flex-direction:column;
    gap:12px;
  }
  h1{ margin:0; font-size:14px; letter-spacing:0.6px; color:#cfe9ff; }
  .status{
    padding:10px;
    border-radius:8px;
    background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    font-size:13px;
  }
  .controls{ display:flex; gap:8px; }
  button{
    background:#11303f;
    border:1px solid rgba(255,255,255,0.03);
    color:#dff6ff;
    padding:8px 10px;
    border-radius:8px;
    cursor:pointer;
    font-size:13px;
  }
  button:active{ transform: translateY(1px); }
  .moves{
    overflow:auto;
    background: linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.00));
    padding:8px;
    border-radius:8px;
    font-family: monospace;
    font-size:13px;
    max-height: calc(var(--board-size) - 160px);
  }
  .footer{
    margin-top:auto;
    font-size:12px;
    color:#9fbbd6;
  }

  /* responsive */
  @media (max-width:920px){
    .app{ flex-direction:column; width:100%; align-items:center; }
    .panel{ width:100%; max-width:var(--board-size); }
  }
</style>
</head>
<body>
<div class="app" role="application" aria-label="Chess game">
  <div id="board" class="board-wrap" aria-hidden="false"></div>

  <aside class="panel" aria-label="Game controls">
    <h1>Single-file Chess</h1>
    <div class="status" id="status">White to move</div>
    <div style="display:flex;gap:8px;">
      <div class="controls" style="flex:1">
        <button id="undoBtn">Undo</button>
        <button id="resetBtn">Reset</button>
      </div>
    </div>

    <div style="display:flex;justify-content:space-between;align-items:center">
      <strong style="font-size:13px">Move history</strong>
      <small id="turnIndicator" style="opacity:0.75">Turn: 1</small>
    </div>
    <div class="moves" id="moves" aria-live="polite"></div>

    <div class="footer">Click a piece to see legal moves. Promotion => Queen.<br>Castling and checks supported.</div>
  </aside>
</div>

<script>
/*
 Single-file chess:
 - Board indexed [0..7][0..7], 0 = rank 8, 7 = rank 1 for display mapping
 - piece objects: {type:'p','r','n','b','q','k', color:'w'|'b'}
 - Uses Unicode chess glyphs for pieces
 - Click-based: click source then destination
 - Enforces legal moves (no move leaving king in check)
 - Supports castling and pawn double-step and promotion (auto to queen)
 - Undo & Reset
 - Move history in simple algebraic-like format
*/

// --- Utilities & glyphs 
const glyphs = {
  w: {k:'♔', q:'♕', r:'♖', b:'♗', n:'♘', p:'♙'},
  b: {k:'♚', q:'♛', r:'♜', b:'♝', n:'♞', p:'♟'}
};
const files = ['a','b','c','d','e','f','g','h'];
const ranks = ['8','7','6','5','4','3','2','1'];

// board state and game metadata
let board = [];
let turn = 'w';
let selected = null;
let legalSquares = [];
let moveHistory = [];
let halfMove = 1;
let castlingRights = { wK:true, wQ:true, bK:true, bQ:true }; // kingside/queenside rights
let enPassantSquare = null; // square index like "e3"
let lastMove = null;

// DOM refs
const boardEl = document.getElementById('board');
const statusEl = document.getElementById('status');
const movesEl = document.getElementById('moves');
const turnIndicator = document.getElementById('turnIndicator');
document.getElementById('resetBtn').addEventListener('click', resetGame);
document.getElementById('undoBtn').addEventListener('click', undoMove);

// initialize board UI
function initBoardUI(){
  boardEl.innerHTML = '';
  for(let r=0;r<8;r++){
    for(let f=0;f<8;f++){
      const sq = document.createElement('div');
      sq.className = 'square ' + (((r+f)%2===0)?'light':'dark');
      sq.dataset.r = r; sq.dataset.f = f;
      sq.id = `sq-${r}-${f}`;
      sq.addEventListener('click', onSquareClick);
      boardEl.appendChild(sq);
    }
  }
}

// algebraic conversions
function rfToCoord(r,f){ return files[f] + ranks[r]; }
function coordToRF(coord){
  if(!coord || coord.length!==2) return null;
  const f = files.indexOf(coord[0]);
  const r = ranks.indexOf(coord[1]);
  if(f<0||r<0) return null;
  return {r, f};
}

// deep copy board
function cloneBoard(b){
  return b.map(row => row.map(cell => cell ? {...cell} : null));
}

// set up initial starting position
function setStartingPosition(){
  board = Array.from({length:8}, ()=>Array(8).fill(null));
  const backRank = ['r','n','b','q','k','b','n','r'];
  // black pieces (row 0 and 1)
  for(let f=0;f<8;f++){
    board[0][f] = {type:backRank[f], color:'b'};
    board[1][f] = {type:'p', color:'b'};
    board[6][f] = {type:'p', color:'w'};
    board[7][f] = {type:backRank[f], color:'w'};
  }
  turn = 'w';
  selected = null;
  legalSquares = [];
  moveHistory = [];
  halfMove = 1;
  castlingRights = { wK:true, wQ:true, bK:true, bQ:true };
  enPassantSquare = null;
  lastMove = null;
  render();
}

// render board pieces and UI highlights
function render(){
  // draw pieces
  for(let r=0;r<8;r++){
    for(let f=0;f<8;f++){
      const el = document.getElementById(`sq-${r}-${f}`);
      el.classList.remove('selected','legal','capture','in-check');
      el.innerHTML = '';
      const cell = board[r][f];
      if(cell){
        const p = document.createElement('span');
        p.className = 'piece';
        p.textContent = glyphs[cell.color][cell.type];
        el.appendChild(p);
      }
    }
  }

  // highlight legal squares
  for(const s of legalSquares){
    const {r,f, capture} = s;
    const el = document.getElementById(`sq-${r}-${f}`);
    if(el) el.classList.add(capture ? 'capture' : 'legal');
  }
  // selected
  if(selected){
    const el = document.getElementById(`sq-${selected.r}-${selected.f}`);
    if(el) el.classList.add('selected');
  }
  // king in check?
  const kingPos = findKing(turn);
  if(kingPos && isSquareAttacked(kingPos.r, kingPos.f, other(turn))){
    const el = document.getElementById(`sq-${kingPos.r}-${kingPos.f}`);
    if(el) el.classList.add('in-check');
  }

  // status text
  const check = isInCheck(turn);
  const mate = isCheckmate(turn);
  statusEl.textContent = mate ? (turn==='w' ? 'Black wins — checkmate' : 'White wins — checkmate') : (check ? `${turn==='w'?'White':'Black'} in check` : `${turn==='w'?'White':'Black'} to move`);
  turnIndicator.textContent = `Turn: ${halfMove}`;
  // move history
  movesEl.innerHTML = '';
  for(let i=0;i<moveHistory.length;i+=2){
    const num = (i/2)+1;
    const w = moveHistory[i] || '';
    const b = moveHistory[i+1] || '';
    const line = document.createElement('div');
    line.textContent = `${num}. ${w} ${b}`;
    movesEl.appendChild(line);
  }
}

// click handler
function onSquareClick(e){
  const r = parseInt(this.dataset.r), f = parseInt(this.dataset.f);
  const cell = board[r][f];

  // if selecting own piece
  if(cell && cell.color === turn){
    selected = {r,f};
    legalSquares = generateLegalMovesForSquare(r,f);
    render();
    return;
  }

  // if already selected and clicking legal target
  if(selected){
    const target = legalSquares.find(s => s.r===r && s.f===f);
    if(target){
      makeMove(selected.r, selected.f, r, f, target.special);
      selected = null;
      legalSquares = [];
      render();
      return;
    }
  }

  // clicked elsewhere: clear selection
  selected = null;
  legalSquares = [];
  render();
}

// other helpers
function other(c){ return c==='w' ? 'b' : 'w'; }

// find king of a color
function findKing(color){
  for(let r=0;r<8;r++) for(let f=0;f<8;f++){
    const c = board[r][f];
    if(c && c.type==='k' && c.color===color) return {r,f};
  }
  return null;
}

// detect if square is attacked by given color
function isSquareAttacked(r,f, byColor){
  // generate pseudo-moves for all opponent pieces and see if any reach r,f
  for(let rr=0; rr<8; rr++){
    for(let ff=0; ff<8; ff++){
      const pc = board[rr][ff];
      if(!pc || pc.color !== byColor) continue;
      const attacks = generatePseudoMovesForSquare(rr,ff, /*forAttackCheck=*/true);
      if(attacks.some(a => a.r===r && a.f===f)) return true;
    }
  }
  return false;
}

// generate moves that ignore check legality (pseudo-legal). For attack checks, pawns attack diagonally not straight.
function generatePseudoMovesForSquare(r,f, forAttackCheck=false){
  const pc = board[r][f];
  if(!pc) return [];
  const moves = [];
  const color = pc.color;
  const dir = color==='w' ? -1 : 1; // row decreases for white forward

  function pushIfEmpty(rr,ff){
    if(rr<0||rr>7||ff<0||ff>7) return;
    if(!board[rr][ff]) moves.push({r:rr,f:ff});
  }
  function pushIfEnemy(rr,ff){
    if(rr<0||rr>7||ff<0||ff>7) return;
    const t = board[rr][ff];
    if(t && t.color!==color) moves.push({r:rr,f:ff, capture:true});
  }

  switch(pc.type){
    case 'p':
      // pawn moves: forward if empty (not for attackCheck)
      const one = r + dir;
      if(!forAttackCheck){
        if(one>=0 && one<=7 && !board[one][f]){
          moves.push({r:one,f});
          // double-step
          const startRow = (color==='w')?6:1;
          const two = r + 2*dir;
          if(r === startRow && !board[two][f]) moves.push({r:two,f, special:'double'});
        }
      }
      // attacks
      for(const ff of [f-1,f+1]){
        const rr = r + dir;
        if(rr<0||rr>7||ff<0||ff>7) continue;
        if(board[rr][ff] && board[rr][ff].color!==color) moves.push({r:rr,f:ff, capture:true});
        // en passant possibility
        if(enPassantSquare){
          const ep = coordToRF(enPassantSquare);
          if(ep && ep.r===rr && ep.f===ff) moves.push({r:rr,f:ff, capture:true, special:'enpassant'});
        }
        // when generating attack-targets (for check detection), pawns attack diagonally regardless of occupancy
        if(forAttackCheck && !board[rr][ff]){
          // pawns can still attack an empty square (used to detect checks)
          moves.push({r:rr,f:ff});
        }
      }
      break;

    case 'n':
      const nMoves = [[-2,-1],[-2,1],[-1,-2],[-1,2],[1,-2],[1,2],[2,-1],[2,1]];
      for(const [dr,df] of nMoves){
        const rr = r+dr, ff = f+df;
        if(rr<0||rr>7||ff<0||ff>7) continue;
        if(!board[rr][ff]) moves.push({r:rr,f:ff});
        else if(board[rr][ff].color!==color) moves.push({r:rr,f:ff, capture:true});
      }
      break;

    case 'b':
    case 'r':
    case 'q':
      const dirs = [];
      if(pc.type==='b' || pc.type==='q'){
        dirs.push([-1,-1],[-1,1],[1,-1],[1,1]);
      }
      if(pc.type==='r' || pc.type==='q'){
        dirs.push([-1,0],[1,0],[0,-1],[0,1]);
      }
      for(const [dr,df] of dirs){
        let rr=r+dr, ff=f+df;
        while(rr>=0 && rr<=7 && ff>=0 && ff<=7){
          if(!board[rr][ff]) {
            moves.push({r:rr,f:ff});
          } else {
            if(board[rr][ff].color!==color) moves.push({r:rr,f:ff, capture:true});
            break;
          }
          rr+=dr; ff+=df;
        }
      }
      break;

    case 'k':
      for(let dr=-1; dr<=1; dr++){
        for(let df=-1; df<=1; df++){
          if(dr===0 && df===0) continue;
          const rr=r+dr, ff=f+df;
          if(rr<0||rr>7||ff<0||ff>7) continue;
          if(!board[rr][ff]) moves.push({r:rr,f:ff});
          else if(board[rr][ff].color!==color) moves.push({r:rr,f:ff, capture:true});
        }
      }
      // castling (only in non-attack scanning)
      if(!forAttackCheck){
        if(color==='w' && r===7 && f===4){
          // kingside
          if(castlingRights.wK && !board[7][5] && !board[7][6] && !isSquareAttacked(7,4,other(color)) && !isSquareAttacked(7,5,other(color)) && !isSquareAttacked(7,6,other(color))){
            moves.push({r:7,f:6, special:'castleK'});
          }
          // queenside
          if(castlingRights.wQ && !board[7][1] && !board[7][2] && !board[7][3] && !isSquareAttacked(7,4,other(color)) && !isSquareAttacked(7,3,other(color)) && !isSquareAttacked(7,2,other(color))){
            moves.push({r:7,f:2, special:'castleQ'});
          }
        }
        if(color==='b' && r===0 && f===4){
          if(castlingRights.bK && !board[0][5] && !board[0][6] && !isSquareAttacked(0,4,other(color)) && !isSquareAttacked(0,5,other(color)) && !isSquareAttacked(0,6,other(color))){
            moves.push({r:0,f:6, special:'castleK'});
          }
          if(castlingRights.bQ && !board[0][1] && !board[0][2] && !board[0][3] && !isSquareAttacked(0,4,other(color)) && !isSquareAttacked(0,3,other(color)) && !isSquareAttacked(0,2,other(color))){
            moves.push({r:0,f:2, special:'castleQ'});
          }
        }
      }
      break;
  }
  return moves;
}

// generate legal moves for a square (i.e., moves that don't leave own king in check)
function generateLegalMovesForSquare(r,f){
  const pc = board[r][f];
  if(!pc) return [];
  const pseudo = generatePseudoMovesForSquare(r,f);
  const legal = [];
  for(const mv of pseudo){
    // make a temporary copy and apply move
    const save = {
      board: cloneBoard(board),
      castling: {...castlingRights},
      enPassant: enPassantSquare,
      lastMove
    };
    const captured = makeMoveOnBoard(r,f,mv.r,mv.f, mv.special, /*applyToRealBoard=*/false);
    // if king of moving color not in check, it's legal
    const kingPos = findKing(pc.color);
    const inCheck = kingPos ? isSquareAttacked(kingPos.r, kingPos.f, other(pc.color)) : true;
    // revert board
    board = save.board;
    castlingRights = save.castling;
    enPassantSquare = save.enPassant;
    lastMove = save.lastMove;
    if(!inCheck) {
      // annotate capture when target square had an enemy OR enpassant
      const targetCell = board[mv.r][mv.f];
      const wasCapture = mv.capture || (mv.special==='enpassant') || (targetCell && targetCell.color !== pc.color);
      legal.push({...mv, capture: wasCapture});
    }
  }
  return legal;
}

// apply move to the board. If applyToRealBoard=false, apply on cloned board and don't modify global metadata (used by generator)
function makeMoveOnBoard(fromR, fromF, toR, toF, special, applyToRealBoard=true){
  // returns captured piece or info
  const moving = board[fromR][fromF];
  if(!moving) return null;
  const target = board[toR][toF];
  let captured = target ? {...target} : null;

  // handle en passant capture
  if(special === 'enpassant'){
    const dir = moving.color==='w' ? 1 : -1;
    const capR = toR + dir;
    captured = {...board[capR][toF]};
    board[capR][toF] = null;
  }

  // perform move
  board[toR][toF] = moving;
  board[fromR][fromF] = null;

  // pawn promotion: if pawn reaches last rank -> promote to queen
  if(moving.type === 'p'){
    const lastRank = moving.color==='w' ? 0 : 7;
    if(toR === lastRank){
      board[toR][toF] = {type:'q', color:moving.color};
    }
  }

  // castling moves: move rook as well
  if(special === 'castleK'){
    // kingside: rook moves from f=7 to f=5 (same row)
    const row = moving.color==='w' ? 7 : 0;
    const rook = board[row][7];
    board[row][5] = rook;
    board[row][7] = null;
  } else if(special === 'castleQ'){
    const row = moving.color==='w' ? 7 : 0;
    const rook = board[row][0];
    board[row][3] = rook;
    board[row][0] = null;
  }

  if(applyToRealBoard){
    // update castling rights if king or rooks moved or captured
    if(moving.type==='k'){
      if(moving.color==='w'){ castlingRights.wK=false; castlingRights.wQ=false; }
      else { castlingRights.bK=false; castlingRights.bQ=false; }
    }
    if(moving.type==='r'){
      if(fromR===7 && fromF===0) castlingRights.wQ=false;
      if(fromR===7 && fromF===7) castlingRights.wK=false;
      if(fromR===0 && fromF===0) castlingRights.bQ=false;
      if(fromR===0 && fromF===7) castlingRights.bK=false;
    }
    // if rook was captured, update opponent's castling rights
    if(captured && captured.type==='r'){
      if(toR===7 && toF===0) castlingRights.wQ=false;
      if(toR===7 && toF===7) castlingRights.wK=false;
      if(toR===0 && toF===0) castlingRights.bQ=false;
      if(toR===0 && toF===7) castlingRights.bK=false;
    }

    // en passant handling
    if(special === 'double'){
      enPassantSquare = rfToCoord((fromR + toR)/2|0, fromF);
    } else {
      enPassantSquare = null;
    }

    lastMove = {from:{r:fromR,f:fromF}, to:{r:toR,f:toF}, piece:moving, captured, special};
  }

  return captured;
}

// make move in game (with history, turn switching)
function makeMove(fromR, fromF, toR, toF, special){
  const moving = board[fromR][fromF];
  if(!moving || moving.color !== turn) return;
  // store snapshot for undo
  const snapshot = {
    board: cloneBoard(board),
    castling: {...castlingRights},
    enPassant: enPassantSquare,
    lastMove,
    turn,
    halfMove
  };

  // perform move
  const captured = makeMoveOnBoard(fromR, fromF, toR, toF, special, true);

  // add to move history in a simple notation
  const san = formatMoveSan(moving, fromR,fromF, toR,toF, captured, special);
  if(turn==='w') moveHistory.push(san); else moveHistory.push(san);
  if(turn==='b') halfMove++;

  // switch turn
  turn = other(turn);
  // if a king moved, we already updated castling rights in makeMoveOnBoard
  render();

  // record snapshot attached to lastMove for undo
  lastMove = { snapshot, move: {from:{r:fromR,f:fromF}, to:{r:toR,f:toF}, special, san} };
  // check for stalemate or checkmate
  if(isCheckmate(turn)){
    // game over, handled in render's status message
  } else if(isStalemate(turn)){
    statusEl.textContent = "Draw — stalemate";
  }
}

// simple SAN-ish formatting (not full SAN): e.g., Nf3, exd5, O-O
function formatMoveSan(piece, fromR,fromF,toR,toF, captured, special){
  if(special==='castleK' || special==='castleQ'){
    return special==='castleK' ? 'O-O' : 'O-O-O';
  }
  const pieceLetter = piece.type==='p' ? '' : piece.type.toUpperCase();
  const captureMark = captured ? 'x' : '';
  const dest = rfToCoord(toR,toF);
  // disambiguation skipped for simplicity
  return `${pieceLetter}${captureMark}${dest}`;
}

// check if side is in check
function isInCheck(side){
  const king = findKing(side);
  if(!king) return false;
  return isSquareAttacked(king.r, king.f, other(side));
}

// checkmate detection: if in check and has no legal moves
function isCheckmate(side){
  if(!isInCheck(side)) return false;
  // if any legal move exists, not checkmate
  for(let r=0;r<8;r++){
    for(let f=0;f<8;f++){
      const c = board[r][f];
      if(c && c.color===side){
        const legal = generateLegalMovesForSquare(r,f);
        if(legal.length>0) return false;
      }
    }
  }
  return true;
}

// stalemate detection
function isStalemate(side){
  if(isInCheck(side)) return false;
  for(let r=0;r<8;r++){
    for(let f=0;f<8;f++){
      const c = board[r][f];
      if(c && c.color===side){
        const legal = generateLegalMovesForSquare(r,f);
        if(legal.length>0) return false;
      }
    }
  }
  return true;
}

// generate legal moves wrapper used by UI
function generateLegalMovesForSquareUI(r,f){
  const legal = generateLegalMovesForSquare(r,f);
  // convert to include capture info if landing square has enemy
  return legal.map(m => {
    return {...m, capture: m.capture || (board[m.r][m.f] && board[m.r][m.f].color !== board[r][f].color)};
  });
}

// We want UI generator that uses current enPassant/castling etc; expose properly
function generateLegalMovesForSquarePublic(r,f){
  return generateLegalMovesForSquare(r,f);
}

// Undo last move
function undoMove(){
  if(!lastMove || !lastMove.snapshot) return;
  const snap = lastMove.snapshot;
  board = cloneBoard(snap.board);
  castlingRights = {...snap.castling};
  enPassantSquare = snap.enPassant;
  lastMove = null;
  turn = snap.turn;
  halfMove = snap.halfMove;
  // remove last SAN from moveHistory
  moveHistory.pop();
  render();
}

// Reset
function resetGame(){
  setStartingPosition();
}

// bootstrap UI and game
initBoardUI();
setStartingPosition();

// expose some functions for internal use
// Hook to use global generate moves (for selection highlighting)
function generateLegalMovesForSquare(r,f){
  return generateLegalMovesForSquarePublic(r,f);
}

// Replace earlier generateLegalMovesForSquare with the UI-friendly one
// But since we've already defined it, it's okay.

</script>
</body>
</html>
