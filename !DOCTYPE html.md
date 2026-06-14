#   
  
  
  
  
  
<!DOCTYPE html>  
  
<html lang="en">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>My Watch List</title>  
<style>  
  :root {  
    --bg: #0d0d0f;  
    --surface: #16161a;  
    --surface2: #1e1e24;  
    --border: rgba(255,255,255,0.08);  
    --border2: rgba(255,255,255,0.14);  
    --text: #f0f0f0;  
    --muted: #888;  
    --accent: #e85d4a;  
    --accent2: #ff8c7a;  
    --done: #4caf82;  
    --watching: #e8a93a;  
    --dropped: #e85d4a;  
    --radius: 10px;  
    --radius-sm: 6px;  
  }  
  * { box-sizing: border-box; margin: 0; padding: 0; }  
  body { background: var(--bg); color: var(--text); font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; font-size: 15px; min-height: 100vh; }  
  
  
header { padding: 2rem 1.5rem 1rem; border-bottom: 1px solid var(--border); }   
header h1 { font-size: 1.6rem; font-weight: 700; letter-spacing: -0.5px; color: var(--accent2); }   
header p { color: var(--muted); font-size: 13px; margin-top: 4px; }  
  
.stats { display: flex; gap: 10px; padding: 1rem 1.5rem; border-bottom: 1px solid var(--border); flex-wrap: wrap; }   
.stat { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 8px 14px; font-size: 12px; }   
.stat span { display: block; font-size: 18px; font-weight: 700; margin-bottom: 1px; }   
.stat.s-done span { color: var(--done); }   
.stat.s-watch span { color: var(--watching); }   
.stat.s-drop span { color: var(--dropped); }   
.stat.s-total span { color: var(--accent2); }  
  
.controls { padding: 1rem 1.5rem; display: flex; gap: 8px; flex-wrap: wrap; border-bottom: 1px solid var(--border); position: sticky; top: 0; background: var(--bg); z-index: 10; }   
.controls input, .controls select {   
background: var(--surface); border: 1px solid var(--border2); border-radius: var(--radius-sm);   
color: var(--text); padding: 8px 12px; font-size: 13px; outline: none;   
}   
.controls input { flex: 1; min-width: 180px; }   
.controls input:focus, .controls select:focus { border-color: var(--accent); }   
.controls select option { background: #1e1e24; }  
  
.result-count { padding: 8px 1.5rem; font-size: 12px; color: var(--muted); border-bottom: 1px solid var(--border); }  
  
.list { padding: 0.5rem 1rem 4rem; display: flex; flex-direction: column; gap: 6px; }  
  
.card {   
background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius);   
padding: 12px 14px; cursor: pointer; transition: border-color 0.15s;   
}   
.card:hover { border-color: var(--border2); }   
.card.open { border-color: var(--accent); }  
  
.card-top { display: flex; align-items: flex-start; gap: 10px; }   
.card-num { font-size: 11px; color: var(--muted); min-width: 26px; padding-top: 2px; }   
.card-main { flex: 1; min-width: 0; }   
.card-title { font-weight: 600; font-size: 14px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }   
.card-orig { font-size: 11px; color: var(--muted); margin-top: 2px; }   
.card-right { display: flex; flex-direction: column; align-items: flex-end; gap: 5px; flex-shrink: 0; }  
  
.status-pill { font-size: 11px; font-weight: 600; padding: 3px 9px; border-radius: 99px; }   
.sp-done    { background: rgba(76,175,130,0.15); color: var(--done); }   
.sp-watching{ background: rgba(232,169,58,0.15); color: var(--watching); }   
.sp-dropped { background: rgba(232,93,74,0.15);  color: var(--dropped); }  
  
.type-pill { font-size: 10px; padding: 2px 7px; border-radius: 99px; background: var(--surface2); color: var(--muted); border: 1px solid var(--border); }  
  
.btn-edit {   
background: none; border: none; color: var(--muted); cursor: pointer;   
font-size: 11px; font-weight: 600; text-decoration: underline; padding: 2px 4px;   
transition: color 0.1s;   
}   
.btn-edit:hover { color: var(--accent2); }  
  
.card-body { display: none; margin-top: 12px; padding-top: 12px; border-top: 1px solid var(--border); }   
.card.open .card-body { display: block; }  
  
.card-progress { font-size: 12px; color: var(--watching); margin-bottom: 8px; }   
.card-summary { font-size: 13px; color: #ccc; line-height: 1.6; margin-bottom: 10px; }   
.genres { display: flex; flex-wrap: wrap; gap: 5px; }   
.genre-tag { font-size: 11px; padding: 3px 9px; border-radius: 99px; background: var(--surface2); color: var(--muted); border: 1px solid var(--border); }  
  
/* Edit Form Popdown Section */   
.edit-panel { display: none; margin-top: 12px; padding-top: 12px; border-top: 1px dashed var(--border2); cursor: default; }   
.card.edit-open .edit-panel { display: block; }  
  
.edit-grid { display: flex; flex-direction: column; gap: 10px; margin-bottom: 12px; }   
.edit-field { display: flex; flex-direction: column; gap: 4px; }   
.edit-field label { font-size: 11px; color: var(--muted); font-weight: 600; text-transform: uppercase; }   
.edit-field input, .edit-field select {   
background: var(--surface2); border: 1px solid var(--border2); border-radius: var(--radius-sm);   
color: var(--text); padding: 6px 10px; font-size: 13px; outline: none;   
}   
.edit-field input:focus, .edit-field select:focus { border-color: var(--accent); }   
.edit-actions { display: flex; gap: 8px; justify-content: flex-end; }   
.btn-action {   
padding: 6px 12px; font-size: 12px; font-weight: 600; border-radius: var(--radius-sm); cursor: pointer; border: none;   
}   
.btn-save { background: var(--done); color: #fff; }   
.btn-save:hover { opacity: 0.9; }   
.btn-cancel { background: transparent; color: var(--muted); border: 1px solid var(--border2); }   
.btn-cancel:hover { background: rgba(255,255,255,0.03); color: var(--text); }  
  
.fixed-note { font-size: 11px; color: var(--done); margin-top: 4px; }  
  
@media (min-width: 600px) {   
.list { padding: 0.75rem 1.5rem 4rem; }   
.card-top { gap: 14px; }   
.card-title { font-size: 15px; white-space: normal; }   
.edit-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }   
.edit-field.full-width { grid-column: span 2; }   
}   
</style>  
</head>  
<body>  
  
  
<header>  
  <h1>My Watch List</h1>  
  <p>Tap any title to see genres &amp; summary</p>  
</header>  
  
  
<div class="stats">  
  <div class="stat s-total"><span id="st-total">0</span>Total</div>  
  <div class="stat s-done"><span id="st-done">0</span>Done</div>  
  <div class="stat s-watch"><span id="st-watch">0</span>Watching</div>  
  <div class="stat s-drop"><span id="st-drop">0</span>Dropped</div>  
</div>  
  
  
<div class="controls">  
  <input type="search" id="search" placeholder="Search titles, genres, summaries…" oninput="filter()">  
  <select id="fStatus" onchange="filter()">  
    <option value="">All statuses</option>  
    <option value="done">Done</option>  
    <option value="watching">Watching</option>  
    <option value="dropped">Dropped</option>  
  </select>  
  <select id="fType" onchange="filter()">  
    <option value="">All types</option>  
    <option value="Anime">Anime</option>  
    <option value="Movie">Movie</option>  
    <option value="Manga">Manga</option>  
    <option value="Special">Special</option>  
  </select>  
  <select id="fGenre" onchange="filter()">  
    <option value="">All genres</option>  
    <option>Action</option><option>Adventure</option><option>Comedy</option><option>Drama</option>  
    <option>Fantasy</option><option>Horror</option><option>Mecha</option><option>Music</option>  
    <option>Mystery</option><option>Psychological</option><option>Romance</option>  
    <option>Sci-Fi</option><option>Slice of Life</option><option>Sports</option>  
    <option>Supernatural</option><option>Thriller</option>  
  </select>  
</div>  
  
  
<div class="result-count" id="rcount"></div>  
<div class="list" id="list"></div>  
  
  
<script>  
const ALL = [  
  {n:1,  orig:"Great pretender",                    title:"Great Pretender",                           fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Action","Comedy","Thriller"],               summary:"A Japanese con artist joins forces with a flamboyant international swindler, pulling off elaborate heists targeting criminals and corrupt elites around the world. Each arc is a self-contained caper packed with twists, vibrant locations, and constantly shifting alliances."},  
  {n:2,  orig:"Bestars",                             title:"Beastars",                                  fixed:true,  type:"Anime",   status:"done",     progress:"",            genres:["Drama","Thriller","Romance"],               summary:"In a world of anthropomorphic animals where carnivores and herbivores coexist uneasily, a shy wolf named Legoshi wrestles with his predatory instincts while falling for a small dwarf rabbit. The series digs into identity, desire, and social prejudice through its deeply unusual cast."},  
  {n:3,  orig:"Erased",                              title:"Erased",                                    fixed:false, type:"Anime",   status:"watching", progress:"Ep 10",       genres:["Mystery","Thriller","Drama"],               summary:"A struggling manga artist with an involuntary ability to leap back in time moments before tragedy strikes is sent all the way back to his childhood after a catastrophe at home. He must prevent a string of childhood kidnappings to save the people he loves."},  
  {n:4,  orig:"The way of the house husband",        title:"The Way of the Househusband",               fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Comedy","Drama","Action"],                  summary:"A legendary yakuza boss, feared across the criminal underworld, has retired to become a devoted stay-at-home husband. His terrifying skills and reputation collide hilariously with the mundane demands of grocery shopping and cooking."},  
  {n:5,  orig:"Your lie in April",                   title:"Your Lie in April",                         fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Romance","Drama","Music"],                  summary:"A piano prodigy who lost the ability to hear his own playing after his mother's death is shaken back to life by a vivacious violinist who bursts into his colorless world. Their intertwined performances push him to rediscover music and what it means to live."},  
  {n:6,  orig:"Highrise invasion",                   title:"High-Rise Invasion",                        fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Action","Horror","Thriller"],               summary:"A high school girl is trapped in a surreal world of skyscrapers connected only by rooftops, hunted by masked killers programmed to push victims to their deaths. She fights to survive and find her missing brother while uncovering the dark purpose behind this twisted realm."},  
  {n:7,  orig:"A silent voice",                      title:"A Silent Voice",                            fixed:false, type:"Movie",   status:"done",     progress:"",            genres:["Drama","Romance","Slice of Life"],          summary:"A former bully who tormented a deaf classmate in elementary school spends his teenage years consumed by guilt and social isolation before deciding to seek her forgiveness. The film is a deeply emotional reckoning with bullying, redemption, and human connection."},  
  {n:8,  orig:"One punch man",                       title:"One Punch Man",                             fixed:false, type:"Anime",   status:"watching", progress:"S2 Ep 5",     genres:["Action","Comedy","Sci-Fi"],                 summary:"A superhero named Saitama can obliterate any enemy with a single punch, yet finds himself crushingly bored by the total absence of a real challenge. The series gleefully skewers superhero tropes as he drifts through rankings, monsters, and peers who take themselves far too seriously."},  
  {n:9,  orig:"Jordan the princess of snow and blood", title:"Jouran: The Princess of Snow and Blood", fixed:true,  type:"Anime",   status:"watching", progress:"Ep 2",        genres:["Action","Fantasy","Drama"],                 summary:"In an alternate Meiji-era Japan still ruled by the Tokugawa Shogunate, a young woman with a rare bloodline power joins a covert executioner unit hunting traitors. Her mission is inseparable from a burning personal vendetta against the man who annihilated her family."},  
  {n:10, orig:"Odtaxi",                              title:"Odd Taxi",                                  fixed:true,  type:"Anime",   status:"watching", progress:"",            genres:["Mystery","Drama","Thriller"],               summary:"A walrus taxi driver with a gift for dry, probing conversation finds his quiet life entangled with a missing girl case and a web of strangers each carrying their own secrets. The slow-burn mystery masterfully braids multiple storylines into a deeply satisfying whole."},  
  {n:11, orig:"Rising of the shield hero",           title:"The Rising of the Shield Hero",             fixed:false, type:"Anime",   status:"watching", progress:"S4 Ep 1",     genres:["Fantasy","Action","Drama"],                 summary:"An ordinary Japanese man is summoned to a medieval fantasy world as one of four legendary heroes, then immediately betrayed and branded a criminal, left with only a shield and a slave girl as allies. He rebuilds his power and reputation from scratch while battling Waves of catastrophe and a hostile kingdom."},  
  {n:12, orig:"If her flag breaks",                  title:"If Her Flag Breaks",                        fixed:false, type:"Anime",   status:"watching", progress:"Ep 3",        genres:["Comedy","Romance","Fantasy"],               summary:"A boy who can see flags above people's heads predicting fate — death flags, romance flags — frantically tries to break the dangerous ones around him. He ends up at the center of an ever-growing household of girls, each with flags that demand careful management."},  
  {n:13, orig:"Date a live",                         title:"Date A Live",                               fixed:false, type:"Anime",   status:"watching", progress:"Ep 5",        genres:["Comedy","Romance","Sci-Fi"],                summary:"Mysterious beings called Spirits tear open the sky and cause catastrophic destruction when they appear, but a high school boy learns the only way to neutralize them is to make them fall in love with him. Guided by a real-time dating sim, he goes on earnest dates with world-threatening entities."},  
  {n:14, orig:"Overlord",                            title:"Overlord",                                  fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Fantasy","Action","Comedy"],                summary:"A veteran gamer logged in during a shutting-down MMORPG's final moments finds himself transported inside the game as his powerful undead sorcerer, surrounded by fully sentient NPCs. Rather than searching for a way home, he sets about conquering this strange new world while suppressing the last fragments of his human empathy."},  
  {n:15, orig:"Darling in the franxx",               title:"Darling in the FranXX",                     fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Sci-Fi","Romance","Mecha"],                 summary:"In a ravaged future, children called Parasites are bred solely to pilot giant mechs in male-female pairs, knowing nothing of the world beyond their cages. When a mysterious horned girl joins one squad, she ignites questions about humanity, freedom, and what the children were never meant to know."},  
  {n:16, orig:"Black clover",                        title:"Black Clover",                              fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Action","Fantasy","Comedy"],                summary:"In a world where magic determines everything, a boy born with absolutely zero magical power refuses to abandon his dream of becoming the Wizard King. Armed with a rare five-leaf clover grimoire and bottomless stubbornness, he claws his way up from nothing alongside his rival."},  
  {n:17, orig:"Terror in Renaissance",               title:"Terror in Resonance",                       fixed:true,  type:"Anime",   status:"watching", progress:"Ep 4",        genres:["Thriller","Mystery","Drama"],               summary:"Two teenage boys who escaped from a clandestine government experiment begin planting bombs across Tokyo as Nine and Twelve, leaving cryptic mythological riddles for police to solve. A brilliant, world-weary detective closes in, slowly unraveling a conspiracy buried deep in Japan's past."},  
  {n:18, orig:"Tokyo ghoul szn 3",                   title:"Tokyo Ghoul",                               fixed:false, type:"Anime",   status:"watching", progress:"S3 Ep 11",    genres:["Action","Horror","Drama"],                  summary:"A college student's life is irrevocably shattered when a chance encounter with a flesh-eating ghoul leaves him transformed into a half-ghoul hybrid. Caught between two worlds and hunted by both, he fights to hold onto his humanity while confronting the monster he is becoming."},  
  {n:19, orig:"The promised Neverland",              title:"The Promised Neverland",                    fixed:false, type:"Anime",   status:"dropped",  progress:"",            genres:["Horror","Mystery","Thriller"],              summary:"Three gifted orphans living in an idyllic countryside orphanage uncover a nightmarish truth about their existence and the fate awaiting every child there. The series becomes a nerve-shredding chess match between the children and their seemingly perfect caretaker as they plan an impossible escape."},  
  {n:20, orig:"Love is war",                         title:"Kaguya-sama: Love Is War",                  fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Comedy","Romance","Drama"],                 summary:"The president and vice president of an elite school's student council are both deeply in love but far too proud to confess first, so each wages elaborate psychological warfare to force the other's hand. Beneath the absurd comedy is a genuinely tender romance that earns every moment."},  
  {n:21, orig:"Konosuba",                            title:"KonoSuba",                                  fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Comedy","Fantasy","Adventure"],             summary:"A boy who dies a deeply embarrassing death is reincarnated in a fantasy world and assembles a party of gloriously useless companions — a praise-hungry goddess, an explosion-obsessed mage, and a masochistic crusader. The series is a joyful parody of isekai tropes that thrives on the chaos of its loveable disasters."},  
  {n:22, orig:"No game no life",                     title:"No Game No Life",                           fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Fantasy","Comedy","Sci-Fi"],                summary:"A reclusive genius sibling duo with an unbroken gaming record are transported to a world where all conflict — from politics to war — is resolved through games. Armed with near-superhuman intellect and borderline-illegal strategies, they set out to conquer every race and challenge God himself."},  
  {n:23, orig:"Dr stone",                            title:"Dr. Stone",                                 fixed:false, type:"Anime",   status:"watching", progress:"S3 Ep 5",     genres:["Sci-Fi","Adventure","Comedy"],              summary:"After a mysterious phenomenon petrifies all of humanity for thousands of years, a teenage science genius awakens and methodically rebuilds civilization from scratch, starting with fire and working toward modern technology. His contagious enthusiasm for discovery drives one of anime's most inventive premises."},  
  {n:24, orig:"Love chinnubio and other delusions",  title:"Love, Chunibyo & Other Delusions",          fixed:true,  type:"Anime",   status:"dropped",  progress:"",            genres:["Comedy","Romance","Drama"],                 summary:"A high schooler desperate to leave behind his embarrassing middle-school fantasy phase meets a girl who still lives completely inside her own delusion of dark powers and ancient contracts. Their relationship gently explores why people cling to imagined worlds rather than face an ordinary reality."},  
  {n:25, orig:"Dragon maid",                         title:"Miss Kobayashi's Dragon Maid",              fixed:false, type:"Anime",   status:"dropped",  progress:"",            genres:["Comedy","Fantasy","Slice of Life"],         summary:"An overworked programmer drunkenly invites a dragon from another dimension to live with her, and the dragon shows up the next morning ready to be her devoted maid. The warm slice-of-life comedy expands as more fantastical beings wander into the human world and find themselves reluctant to leave."},  
  {n:26, orig:"Fate zero",                           title:"Fate/Zero",                                 fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Action","Fantasy","Thriller"],              summary:"Seven mages each summon a legendary hero from history or myth to fight to the death in a secret war for the Holy Grail, which promises to grant any wish. Fate/Zero is a prequel with a darker, more morally complex lens than its successors, asking what people are truly willing to sacrifice for their ideals."},  
  {n:27, orig:"Akame ga kill",                       title:"Akame ga Kill!",                            fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Action","Fantasy","Drama"],                 summary:"A naive country boy travels to the capital to fund his impoverished village and instead joins a band of assassins fighting to bring down a deeply corrupt empire. The series kills its characters without mercy, making every battle feel genuinely dangerous and raising the emotional stakes at every turn."},  
  {n:28, orig:"My hero academia",                    title:"My Hero Academia",                          fixed:false, type:"Anime",   status:"watching", progress:"S5",          genres:["Action","Fantasy","Drama"],                 summary:"In a world where most people are born with superpowers called Quirks, a boy born with nothing dreams of becoming the greatest hero of all. A life-changing encounter with the top hero passes him a legendary power, and he enrolls in a prestigious hero academy where the road ahead is longer and harder than he imagined."},  
  {n:29, orig:"Highschool dxd",                      title:"High School DxD",                           fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Comedy","Fantasy","Action"],                summary:"A high school boy is murdered on his first date and resurrected as a devil by a stunning upperclassman, plunging him into a secret world of warring supernatural factions. He joins her household, aims to become a powerful devil himself, and ends up embroiled in increasingly outrageous battles with angels and rival devils."},  
  {n:30, orig:"Death note",                          title:"Death Note",                                fixed:false, type:"Anime",   status:"dropped",  progress:"",            genres:["Mystery","Thriller","Psychological"],       summary:"A genius student who finds a supernatural notebook capable of killing anyone whose name is written in it begins secretly executing criminals, styling himself as a divine judge of humanity. What unfolds is one of anime's greatest cat-and-mouse duels as the world's top detective closes in with methodical brilliance."},  
  {n:31, orig:"Evangelion",                          title:"Neon Genesis Evangelion",                   fixed:true,  type:"Anime",   status:"done",     progress:"",            genres:["Mecha","Sci-Fi","Psychological"],           summary:"Teenagers are recruited to pilot massive bio-mechanical mecha against alien entities called Angels threatening what remains of humanity after a global catastrophe. Beneath the breathtaking battles is a deeply introspective series about depression, identity, and the terror of genuine human connection."},  
  {n:32, orig:"Fire force",                          title:"Fire Force",                                fixed:false, type:"Anime",   status:"watching", progress:"S3 P2 Ep 5",  genres:["Action","Supernatural","Drama"],            summary:"In a world where spontaneous human combustion creates flaming undead monsters, elite squads of pyrokinetic firefighters hunt them down and give the victims a peaceful end. A new recruit with fearsome fire abilities joins the fight while uncovering a conspiracy at the heart of the phenomenon."},  
  {n:33, orig:"Dan Machi",                           title:"Is It Wrong to Try to Pick Up Girls in a Dungeon?", fixed:true, type:"Anime", status:"watching", progress:"S3 Ep 6", genres:["Fantasy","Action","Romance"],          summary:"A novice adventurer scraping by in a dungeon-crawling city catches the attention of a minor goddess and begins a rapid — perhaps impossibly rapid — ascent through the ranks of heroes. The series balances cheerful action with genuine character development as the boy carves his own legend."},  
  {n:34, orig:"The irregular at magic Highschool",   title:"The Irregular at Magic High School",        fixed:false, type:"Anime",   status:"watching", progress:"S2",          genres:["Action","Sci-Fi","Romance"],                summary:"In a future where magic is a regulated technical discipline, a boy dismissed as an academically low-ranked student hides abilities that dwarf everyone around him. The series follows him and his sister through a prestigious magic high school rife with political intrigue and escalating threats."},  
  {n:35, orig:"Sao",                                 title:"Sword Art Online",                          fixed:true,  type:"Anime",   status:"done",     progress:"",            genres:["Action","Fantasy","Romance"],               summary:"Thousands of players log into a revolutionary virtual reality MMORPG and discover they cannot log out — and that death in the game means death in the real world. A solo player with exceptional combat skill must fight his way through one hundred floors to free everyone trapped inside."},  
  {n:36, orig:"Blue exorcist",                       title:"Blue Exorcist",                             fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Action","Supernatural","Fantasy"],          summary:"A teenager discovers that he is the literal son of Satan and possesses demonic powers he never asked for, yet chooses to become an exorcist to fight against demons rather than join them. He enrolls in a school for exorcists while hiding his true lineage from those who would destroy him on sight."},  
  {n:37, orig:"Aot",                                 title:"Attack on Titan",                           fixed:true,  type:"Anime",   status:"watching", progress:"S4 P2 Ep 9",  genres:["Action","Drama","Thriller"],                summary:"Humanity survives behind towering walls, hiding from gigantic humanoid Titans that devour people without reason — until the walls are breached and a young boy watches his world collapse. What begins as a visceral survival story evolves into a sprawling political and moral epic that questions who the real monsters are."},  
  {n:38, orig:"Kill la kill",                        title:"Kill la Kill",                              fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Action","Comedy","Sci-Fi"],                 summary:"A girl transfers to an elite high school ruled by a tyrannical student council in search of her father's killer and discovers a living sailor uniform that grants her extraordinary power. Kill la Kill is a gleefully maximalist action series that never slows down long enough to stop surprising you."},  
  {n:39, orig:"Demon slayer",                        title:"Demon Slayer",                              fixed:false, type:"Anime",   status:"watching", progress:"S2 Ep 6",     genres:["Action","Supernatural","Drama"],            summary:"A kind-hearted boy returns home to find his family slaughtered by a demon and his younger sister transformed into one, and sets out to become a Demon Slayer to find a cure for her. The series is renowned for its stunning animation and emotionally resonant battles."},  
  {n:40, orig:"Food wars",                           title:"Food Wars!",                                fixed:false, type:"Anime",   status:"watching", progress:"S2 Ep 4",     genres:["Comedy","Drama","Action"],                  summary:"A gifted home cook enrolls in Japan's most brutal culinary academy, where students are expelled by the thousands and advancement depends on winning high-stakes cook-offs called Shokugekis. His unconventional recipes and relentless competitive spirit take on the school's rigid elite establishment."},  
  {n:41, orig:"Quintessential quintuplets",          title:"The Quintessential Quintuplets",            fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Comedy","Romance","Drama"],                 summary:"A broke but brilliant high school student takes a tutoring job for a wealthy family's quintuplet daughters, all of whom are failing their classes and deeply resistant to studying. The series is a slow-burn romantic comedy as he navigates five very different personalities and gradually wins each of them over."},  
  {n:42, orig:"Todadora",                            title:"Toradora!",                                 fixed:true,  type:"Anime",   status:"dropped",  progress:"",            genres:["Romance","Comedy","Drama"],                 summary:"A boy with an intimidating face and a tiny, ferocious girl with a reputation for violence discover they each have a crush on the other's best friend and agree to help each other out. The arrangement quickly becomes something neither of them expected in this beloved coming-of-age romance."},  
  {n:43, orig:"Kakeguri",                            title:"Kakegurui",                                 fixed:true,  type:"Anime",   status:"done",     progress:"",            genres:["Thriller","Psychological","Drama"],         summary:"At a prestigious academy where social hierarchy is determined entirely by high-stakes gambling, a transfer student arrives who is utterly fearless — and dangerously addicted to the thrill of risking everything. Her presence upends the school's entire power structure in spectacularly unhinged fashion."},  
  {n:44, orig:"Violet evergarden",                   title:"Violet Evergarden",                         fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Drama","Fantasy","Slice of Life"],          summary:"A former child soldier with mechanical arms who knew nothing but war takes a job as an Auto Memory Doll — a ghostwriter who puts people's unspoken feelings into letters. Each episode is a quiet, devastating portrait of human emotion as Violet slowly learns to understand what love means."},  
  {n:45, orig:"Blood-c",                             title:"Blood-C",                                   fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Action","Horror","Mystery"],                summary:"A cheerful shrine maiden who moonlights as a demon slayer begins to notice disturbing inconsistencies in her picture-perfect village life. As the violence escalates to shocking extremes, the truth of who she is and why she is there unravels in deeply unsettling ways."},  
  {n:46, orig:"Tokyo revengers",                     title:"Tokyo Revengers",                           fixed:false, type:"Anime",   status:"watching", progress:"Ep 19",       genres:["Action","Drama","Thriller"],                summary:"A directionless young man discovers he can time-travel to his middle school years and sets out to prevent the deaths of people he loves by changing the past and stopping a notorious delinquent gang. Each return to the present reveals how drastically — and sometimes worse — his interventions have altered the timeline."},  
  {n:47, orig:"Code Geass",                          title:"Code Geass",                                fixed:false, type:"Anime",   status:"done",     progress:"",            genres:["Mecha","Thriller","Drama"],                 summary:"A exiled prince of a conquered Japan secretly becomes a masked revolutionary leader, wielding a supernatural power that forces absolute obedience in anyone who meets his gaze. He wages a brilliant, morally treacherous campaign against the empire using giant mechs, political manipulation, and ruthless chess-like strategy."},  
  {n:48, orig:"PEAK girl senpai",                    title:"My Senpai is Annoying",                     fixed:true,  type:"Anime",   status:"done",     progress:"",            genres:["Comedy","Romance","Slice of Life"],         summary:"A small but hardworking office woman is constantly teased and fussed over by her large, boisterous senpai at their sales job, much to her exasperation. The series is a warm, low-stakes workplace romance that finds enormous charm in everyday moments between two people who clearly belong together."},  
  {n:49, orig:"Seven deadly sins",                   title:"The Seven Deadly Sins",                     fixed:false, type:"Anime",   status:"watching", progress:"S4 Ep 4",     genres:["Action","Fantasy","Comedy"],                summary:"A princess on the run from a corrupt knighthood seeks out the Seven Deadly Sins, a legendary and disbanded group of the kingdom's most powerful — and most dangerous — knights. Together they uncover a conspiracy targeting the entire realm and prepare for a war that goes far beyond political power."},  
  {n:50, orig:"Parasyte",                            title:"Parasyte: The Maxim",                       fixed:true,  type:"Anime",   status:"done",     progress:"",            genres:["Sci-Fi","Horror","Drama"],                  summary:"Alien parasites fall to Earth and invade human brains, turning their hosts into lethal hunters — but one teenager manages to contain his parasite only in his right hand, forcing them into an uneasy coexistence. Together they navigate a world of grotesque violence while the boy grapples with what remains of his humanity."},  
  {n:51, orig:"Yasuke",                              title:"Yasuke",                                    fixed:false, type:"Anime",   status:"watching", progress:"Ep 1",        genres:["Action","Fantasy","Drama"],                 summary:"Based loosely on the historical African samurai who served under Oda Nobunaga, the series follows Yasuke living in quiet retirement until a child with mysterious powers forces him back into a blood-soaked world of warring factions and dark magic. It  
