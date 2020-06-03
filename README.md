# https-KASE
This is an amazing website to hep you look at the amazing characters of Khan Academy.




//K.A.S.E.


//Remember: All searches are case sensitive!

//This program contains a search engine for Khan Academy Javascript Programs.

//Credit to nameArray printing fix goes to qindiandefence (check out his programs)

//Make an account today!

/** To make this the best possible search engine, I need community submissions for my database, so please submit any program that follows these requirements:
 * 1. It is not a repeat entry. (spin-offs are accepted)
 * 2. The link connects to a Khan Academy program
 * 3. It is your program. Do not submit a program you are not the author of!
 * 
    (Must be in the proper format *CLICK SUBMIT*)
    
 * You are not limited to just one program. Submit as many as you want!

 * See 'About' for more.
*/

var scrollSpeed = [2, 4, 8];
//set this to 0 to turn blur off
var menuBlur = 0;
var popup = false;

//Save here
var userAccount = {
    name: '',
    nameArray: [],
    password: [],
    icon: 0,
    favorites: [],
    history: [],
    setup: false,
};

//Paste this into userAccount and restart to reset the account on this program.
/*
    name: '',
    nameArray: [],
    password: [],
    icon: 0,
    favorites: [],
    history: [],
    setup: false,
*/


/**
 * * Version History
 * 0.0 - initial upload that included search, submit, and login capabilities as well as an about page
 * 0.1 - added DELETE as an alternate to BACKSPACE as well as fixing a printing error for userAccount.nameArray - credit to qindiandefence
 * 0.2 - large update for database; set blur to default 0
 * 1.0 - use indexOf() for better searches - credit to qindiandefence
 * 1.1 - clicking a result opens it in a new tab - credit to Matthew Kestenbaum
 * 1.2 - removed opening new tab due to bugs; fixed error in search results
*/

var customIcon = function(x, y) {
    textAlign(CENTER, CENTER);
    textSize(100);
    fill(255, 0, 0);
    text('C', x + 50, y + 50);
    //must fit inside a 100x100 pixel icon box
};


//Database
//['Name', 'Author', 'Link', 'Tag1', 'Tag2', Tag3']
//If this is a spin-off, update your database by copying this and pasting it into your own spin-off.
var entries = [
    //Matt's Programs
    ['Mini Putt', 'Matt', 'https://www.khanacademy.org/cs/mini-putt/5995007307677696', 'putt', 'golf', 'game'],
    ['BlockFall', 'Matt', 'https://www.khanacademy.org/cs/blockfall/1932026475', 'block', 'fall', 'arcade'],
    ['Prime Calculator', 'Matt', 'https://www.khanacademy.org/cs/prime-calculator/2301087397', 'prime', 'calculator', 'numbers'],
    ['Mr. Pants pants made of Pants', 'Matt', 'https://www.khanacademy.org/cs/mr-pants-pants-made-of-pants/2779106365', 'pants', 'contest entry', 'mr. pants'],
    ['WaveRider', 'Matt', 'https://www.khanacademy.org/cs/waverider/4709453450444800', 'water', 'rider', 'jetski'],
    ['Custom Hole Generator', 'Matt', 'https://www.khanacademy.org/cs/custom-hole-generator/2121990492', 'level creator', 'mini putt', 'hole'],
    ['Jetski', 'Matt', 'https://www.khanacademy.org/cs/jetski/4614379785945088', 'waverider', 'ski', 'water'],
    ['Interactive Graph (sin)', 'Matt', 'https://www.khanacademy.org/cs/interactive-graph-sin/1818045174', 'graph', 'sin', 'numbers'],
    ['Interactive Graph (cos)', 'Matt', 'https://www.khanacademy.org/cs/interactive-graph-cos/1817950409', 'graph', 'cos', 'numbers'],
    ['Interactive Graph (tan)', 'Matt', 'https://www.khanacademy.org/cs/interactive-graph-tan/1820710144', 'graph', 'tan', 'numbers'],
    
    //Khan Academy
    ['Computer Programming', 'khanacademy.org', 'https://www.khanacademy.org/cs', 'programming', 'science', 'khan'],
    ['Browse Programs', 'khanacademy.org', 'https://www.khanacademy.org/cs/browse-programs', 'programs', 'search', 'khan'],
    ['New_Program', 'khanacademy.org', 'https://www.khanacademy.org/cs/new', 'new', 'program', 'khan'],
    ['Documentation', 'khanacademy.org', 'https://www.khanacademy.org/cs/program-docs', 'programming', 'computer', 'khan'],
    ['Community Questions', 'khanacademy.org', 'https://www.khanacademy.org/cs/d', 'question', 'answer', 'khan'],
    
    ['Search Engine (K.A.S.E.)', 'Matt', 'https://www.khanacademy.org/cs/search-engine-kase/2768323021', 'kase', 'search', 'engine'],
    
    ['Magic Leaves (Ad Design)', 'TheUberKid', 'https://khanacademy.org/cs/ ...magic-leaves-ad-design/2576642748', 'ad', 'leaves', 'magic'],
    ['The Ultimate TD', 'the #1 pi proponent', 'https://khanacademy.org/cs/the-ultimate-td/1106141208', 'tower defense', 'game', 'advanced'],
    ['Jumpgirl 2P Split Screen', 'the #1 pi proponent', 'https://khanacademy.org/cs/split-screen/1343205674', 'jumpgirl', '2 player', 'platform'],
    ['Mercury Subspace', 'David Hu', 'https://khanacademy.org/cs/mercury-subspace/938561708', 'space', 'shooter', 'game'],
    ['Water Gun Game', 'the #1 pi proponent', 'https://khanacademy.org/cs/a/1976412530', 'water', 'game', 'shooter'],
    ['The Bug\'s Revenge Series', 'the #1 pi proponent', 'https://khanacademy.org/cs/a/1521016404', 'revenge', 'platform', 'game'],
    ['Mobile Device Jumpgirl', 'the #1 pi proponent', 'https://khanacademy.org/cs/2445436442', 'mobile', 'platform', 'game'],
    ['PieCrafted\'s Account', 'PieCrafted', 'https://khanacademy.org/cs/spin-off-of-search-engin-kase/2779892094', 'piecrafted', '', ''],
    ['Ryan Kee', 'Ryan Kee', 'http://www.khanacademy.org/cs/stickman-adventures/2702550465', 'stickman', 'game', 'cartoon'],
    ['The Star-Spangled Banner', 'Dan the Man', 'https://khanacademy.org/cs/the-star-spangled-banner/2668671300', '', '', ''],
    ['Minion!', 'Josh Mars', 'https://www.khanacademy.org/cs/minion/5856425364422656', 'minion', 'despicableme', 'image'],
    ['Tux', 'Josh Mars', 'https://www.khanacademy.org/cs/tux/2521676971', 'penguin', 'linux', 'logo'],
    ['Ubuntu', 'Josh Mars', 'https://khanacademy.org/cs/ubuntu/2577070907', 'ubuntu', 'linux', 'OS'],
    ['Violin v 1.0', 'Benjamin C. Konczal', 'https://khanacademy.org/cs/violin-v-10/2587061579', 'music', 'instrument', 'violin'],
    ['Old Spice Man\'s Journey', 'Theler', 'https://khanacademy.org/cs/save-the-kidnapped-king/2775022034', 'old spice man', 'kidnapped ', 'journey'],
    ['Error Buddy can Read Your Mind', 'the #1 pi proponent', 'https://khanacademy.org/cs//error-buddy-can-read-your-mind/2269005209', 'math', 'trick', 'error buddy'],
    ['Space Gems', 'the #1 pi proponent', 'https://khanacademy.org/cs/space-gems/1429104882', 'space', 'collect', 'game'],
    ['Khan Academy Star', 'light runner', 'https://khanacademy.org/cs/khan-academy-star/2789598191', 'newspaper', 'star', 'khan academy'],
    ['Haiathien 1.6', 'Gregory', 'https://khanacademy.org/cs/haiathien-16/2583696688', 'game', 'puzzle', 'interactive'],
    ['Exploding Volcano', 'Jared Desai', 'http://khanacademy.org/cs/exploding-volcano-with-circular-explosion/2745286404', 'explosion', 'animation', ''],
    ['Lightsaber Battle', 'Jared Desai', 'http://khanacademy.org/cs/lightsaber-battle/1815644192', 'star wars', 'animation', ''],
    ['Pilgrim', 'Jared Desai', 'http://khanacademy.org/cs/pilgrim-challenge-1/2739996707', 'thanksgiving', 'holiday', 'drawing'],
    ['Creature', 'Leaf', 'http://www.khanacademy.org/cs/project-design-an-animal-wip/2706065376', 'project', 'wip', 'interactive'],
    ['Rocket cube', 'Thomas', 'https://khanacademy.org/cs/rocket-cube/2744796546', 'game', 'platform', ''],
    ['average finder', 'Thomas', 'https://khanacademy.org/cs/average-finder/2666572867', 'Tools', '', ''],
    ['Flight simulator', 'Thomas', 'https://khanacademy.org/cs/flight-simulator-v25/2581586763', 'game', 'simulation', 'flight'],
    ['Breathing Jack', 'BenTheGreat', 'http://www.khanacademy.org/cs/breathing-jack/2236537260', 'Halloween', 'breathing', 'BenTheGreat'],
    ['My House 3D (1st Floor)', 'BenTheGreat', 'http://www.khanacademy.org/cs/my-house-3d-1st-floor/2134572923', '3D', 'House', 'BenTheGreat'],
    ['Magic Hat (Bunny, Hat, AND FACE)', 'BenTheGreat', 'http://www.khanacademy.org/cs/magic-hat-bunny-hat-and-face/6406548507066368', 'Magic', 'Bunny', 'BenTheGreat'],
    ['Potions Fixed By BenTheGreat', 'BenTheGreat', 'http://www.khanacademy.org/cs/potions-by-troy-cookmodified-and-fixed-by-benthegreat/2776976956', 'Potions', 'Graphics', 'BenTheGreat'],
    ['Super Ball Level Pack', 'vyros', 'https://www.khanacademy.org/cs/super-ball-level-pack-17-levels/2077853465', 'game', 'platform', ''],
    ['Xavier', 'Xavier', 'https://khanacademy.org/cs/controlable-truck/1119997086', 'Truck', 'Controlable', ''],
    ['KAPSE(Khan Academy Programs...', 'vyros', 'https://www.khanacademy.org/cs/kapsekhanacademy-programs-search-engine/6566854025805824', 'program', 'search', 'engine'],
    ['Gravity Ball Level Pack', 'vyros', 'https://www.khanacademy.org/cs/gravity-ball-level-pack-10-levels/2094908383', 'program', 'game', 'platform'],
    ['Super Ball Walkthrough', 'vyros', 'https://www.khanacademy.org/cs/super-ball-walkthrough/2279659058', 'program', 'tutorial', 'platform'],
    ['Vyros logo', 'vyros', 'https://www.khanacademy.org/cs/vyros-logo/2272451164', 'program', 'intro', ''],
    ['Merry Christmas ya\'ll', 'Arinj', 'https://khanacademy.org/cs/merry-christmas-yall/2832651457', 'christmas', '', ''],
    ['Falling Snow!', 'Josh Mars', 'https://www.khanacademy.org/cs/falling-snow/4907139659202560', 'snow', 'winter', 'animation'],
    ['Falling Snow!', 'Josh Mars', 'https://www.khanacademy.org/cs/falling-snow/4907139659202560', 'snow', 'winter', 'animation'],
    ['Let\'s stop fighting!', 'RandomPerson16', 'https://khanacademy.org/cs/lets-stop-fighting/2666801380', 'fight', 'winston', 'error-buddy'],
    ['Darth Maul\'s lightsaber! v 1.7', 'Benjamin C. Konczal', 'https://khanacademy.org/cs/darth-mauls-lightsaber-v-17/6376022467411968', 'lightsaber', 'star wars', 'jedi'],
    ['Simplecraft', 'Thomas', 'https://www.khanacademy.org/cs/simplecraft/2864816843', 'minecraft', 'game', ''],
    ['Xavier', 'Xavier', 'https://khanacademy.org/cs/quicktype-19/2803947027', 'word processor', '', ''],
    ['Magic 8-ball', 'Casito64', 'https://khanacademy.org/cs/magic-8-ball/5196900122755072', 'magic 8 ball', '8-ball', '8 ball'],
    ['surprised person', 'Casito64', 'https://khanacademy.org/cs/surprised-person/6264711117012992', 'surprised dude', 'surprised guy', 'surprised man'],
    ['Text Tutorial', '~Makena~', 'https://www.khanacademy.org/cs/text-tutorial/1925783252', '', '', ''],
    ['Supapaint v1.3', 'Treyhat', 'https://khanacademy.org/cs/supa-paint-v13/5921656677597184', 'painting', 'fun', 'drawing program'],
    ['BLOCK the Game', 'A Programmer', 'http://khanacademy.org/cs/block-the-game/5740434842189824', 'block', 'games', ''],
    ['The Elevator', 'A Programmer', 'http://www.khanacademy.org/cs/the-elevator/4824147838369792', 'elevator', 'games', ''],
    ['Daniel Studios', 'A Programmer', 'http://www.khanacademy.org/cs/daniel-studios/5689739661279232', 'danielstudios', 'mainpage', 'main'],
    ['The Khan Club', 'holder.sophia', 'https://khanacademy.org/cs/the-khan-club/4817725991944192', 'contest', 'club', 'team'],
    ['Cookies', 'AirbusA380', 'https://khanacademy.org/cs/cookies/6410014132535296', 'cookies', 'food', 'chocolate'],
    ['Khan Coders', '__STARK__', 'https://khanacademy.org/cs/thekhancoders/6473572560142336', 'Club', 'Code', 'KA'],
    ['Khan Programs', '__STARK__', 'https://www.khanacademy.org/cs/thekhanprograms/6439407913533440', 'Programs', 'Khan', 'Club'],
    ['super snake', 'sleonoras', 'https://khanacademy.org/cs/super-snake/1443679686', 'games', 'snake', 'gems'],['Go Hawks: The Game!', 'Matthew Kestenbaum', 'https://khanacademy.org/cs/go-hawks-the-game/6078725030412288', '350+ votes!', '', ''],
    ['Winston Hunters', 'Matthew Kestenbaum', 'https://khanacademy.org/cs/winstons-hunters-added-more-realistic-blood/4963669655945216', 'Added more realistic blood!', '', ''],
    ['Intake v1.0', 'Matthew Kestenbaum', 'https://khanacademy.org/cs/intake-v10/5161123860971520', 'Super fun!', 'Fast paced!', ''],
    ['Collision', 'Piology', 'https://khanacademy.org/cs/collision/4652526718681088', 'collision', 'ball', 'game'],
    ['3-D Maker', 'AirbusA380', 'https://khanacademy.org/cs/3d-maker/6206715893645312', '3-D', '3-d', ''],
    ['3042 digits of pi','AirbusA380','https://www.khanacademy.org/cs/3042-digits-of-pi/4816302977843200','pi','digits of pi',''],
    ['KA Programmers','https://www.khanacademy.org/cs/ka-programmers/4595113114206208','AirbusA380','team','',''],
    ['Zombie Attack', 'Rocket Scientist I', 'http://www.khanacademy.org/cs/zombies-attack/6065307344961536', 'zombies', 'attack', 'shoot'],
    ['Rocket Scientist Studios', 'Rocket Scientist I', 'http://www.khanacademy.org/cs/rocket-scientist-studios/6523909341970432', 'programs', 'all', 'studios'],
    ['Dogfights of WWII', 'Pikachu', 'https://khanacademy.org/cs/dogfights-of-wwii/2432759261', 'war', 'plane', 'action'],
    ['Computer','AirbusA380','https://www.khanacademy.org/cs/computer/4751110813253632','computer','keyboard','mouse'],
    ['KOPs', 'droidsb', 'https://www.khanacademy.org/cs/ka-outstading-programmers/4536202280566784', 'khan academy programmers', 'kops', 'KOPS'], 
    ['Dummy Bill', 'B.O.B', 'https://khanacademy.org/cs/Dummy-Bill-finished/5669303442472960', 'idk', 'idk', 'idk'],
    ['The Daily Kingdom', 'Treyhat', 'https://khanacademy.org/cs/the-daily-kingdom3/4454018747580416', 'news', 'ads', 'articles'],['The Sword of Calamite', 'the3kidz', 'https://khanacademy.org/cs/the-sword-of-calamite/6631633657528320', 'book', 'contest', 'tsc'],
    ['THE WINSTON LAUNCHER!', 'EnderAndroid', 'https://www.khanacademy.org/cs/the-winston-launcher/5777740175245312', 'winston', 'gun', 'destruction'],
    ["Khan Academy Search Engine/Station","AirbusA380","https://www.khanacademy.org/cs/khan-academy-search-enginestation/6483840492109824","seacrh","programs","search programs"],
    ["Bezier Drawing tool","AirbusA380","https://www.khanacademy.org/cs/bezier-tool/4820301610221568","bezier","shape","tool"],
    ['FIX THE ERRORS With E.B.', 'Island2D', 'https://khanacademy.org/cs/fix-the-errors-with-oh-noes-the-error-buddy/6281170289326432', 'hoppy beaver', 'error buddy', 'side scroller'],
    ['The New Avatars', 'Island2D', 'https://www.khanacademy.org/cs/the-new-avatars/5982763928780800', 'avatars', 'voting', 'new avatars'],
    ['Math test', 'the3kidz', 'https://khanacademy.org/cs/math-test/6722608639770624', 'test', '6th grade', '5th grade'],
    ['Airbus A350 Symbol', 'AirbusA350', 'https://khanacademy.org/cs/s/4849333192097792', 'symbol', 'a350', 'airbus'],
    ["Gun Pakage","GoldenApple","https://khanacademy.org/cs/gun-package-v115824/4919388555706368","simulation","",""],
    ["Bouncing Ball","GoldenApple","https://khanacademy.org/cs/bouncing-ball-v13774/6269414685016064","simulation","2D","ball"],
    ["Click the Circle","GoldenApple","https://khanacademy.org/cs/click-the-circle/6477430593159168","game","trick","simple"],
    ["Static","GoldenApple","https://khanacademy.org/cs/static/6480339977371648","animation","simulation","simple"],
    ['Binary Clock', 'Kendra', 'https://khanacademy.org/cs/binary-clock/5331298741649408', 'binary', 'base 2', 'clock'],
    ['Lies on the Internet!', 'Agent of Awesomeness{Marvel Lover}', 'https://khanacademy.org/cs/lies-on-the-internet/5100669840130048', 'please vote, but you dont have to', '', ''],
    ['Real Speed', 'LightningJimmy', 'https://www.khanacademy.org/cs/real-speed/5070454781378560', 'racing', 'car', ''],
    ['Drawing Animation of a Bus', 'Antonio Sarabia', 'https://khanacademy.org/cs/bus/4898919026262016', 'animation', 'drawing', 'drawing animation'],
    ['Pixelated Bazooka', 'Antonio Sarabia', 'https://khanacademy.org/cs/bazooka/4631166486839296', 'Pixelated ', 'Pixelated Weapon(s)', 'Weapon(s)'],
    ['Winston\'s Story', 'Antonio Sarabia', 'https://khanacademy.org/cs/winstons-story/6708999058096128', 'Avatar', 'Avatar Story', 'Story'],
    ['The Game Competition', 'Antonio Sarabia', 'https://khanacademy.org/cs/the-game-competition/5443240925331456', 'Competition', '', ''],
    ['My Font', 'Antonio Sarabia', 'https://khanacademy.org/cs/my-font/4631464666202112', 'Font', 'Creating your own font', ''],
    ['Pong', 'Ben The Programming Genius', 'http://www.khanacademy.org/cs/pong/6467096657002496', '', '', ''],
    ['How Fast Can You Click Game', 'Ben The Programming Genius', 'https://khanacademy.org/cs/how-fast-can-you-click-game/6102069441724416', '', '', ''],
    ['Canon 2', 'Taha', 'https://www.khanacademy.org/cs/canon-two/6479800897634304', 'canon', '2', 'by taha'],
    ['Calculator', 'Taha', 'https://khanacademy.org/cs/calculator-version-10/6052041994797056', 'calculator', 'calculations', ''],
    ['Cannon 3 ', 'Taha', 'https://khanacademy.org/computer-programming/cannon-3/5168357994397696', 'title', 'progress', 'shooting'],
    ['Guitar Chord Teacher', 'SpongeJr', 'https://khanacademy.org/cs/guitar-chord-teacher/5802803663732736', 'guitar', 'music', 'chord'],
    ['Music Theory Chord Teaching', 'SpongeJr', 'https://khanacademy.org/cs/music-theory-chord-teaching-program/6478611684261888', 'music', 'chord', 'educational'],
    ['Infinity2015\'s Blog | Page 1', 'Infinity2015', 'https://khanacademy.org/cs/infinity2015s-blog-page-1/4679288753487872', 'Blog', '', ''],
    ['Infinity2015\'s Blog | Page 2', 'Infinity2015', 'https://khanacademy.org/cs/infinity2015s-blog-page-2/5658005533360128', 'Blog', '', ''],
];

var iconArray = [
    getImage("avatars/questionmark"), getImage("avatars/leaf-blue"), getImage("avatars/leaf-green"), getImage("avatars/leaf-grey"), getImage("avatars/leaf-orange"), getImage("avatars/leaf-red"), getImage("avatars/leaf-yellow"), getImage("avatars/leafers-seed"), getImage("avatars/leafers-seedling"), getImage("avatars/leafers-sapling"), getImage("avatars/leafers-tree"), getImage("avatars/leafers-ultimate"), getImage("avatars/marcimus"), getImage("avatars/mr-pants"), getImage("avatars/mr-pink"), getImage("avatars/old-spice-man"), getImage("avatars/orange-juice-squid"), getImage("avatars/purple-pi"), getImage("avatars/robot_female_1"), getImage("avatars/robot_female_2"), getImage("avatars/robot_female_3"), getImage("avatars/robot_male_1"), getImage("avatars/robot_male_2"), getImage("avatars/robot_male_3"), getImage("avatars/spunky-sam"), getImage("creatures/Hopper-Happy"), getImage("creatures/Hopper-Cool"), getImage("creatures/Hopper-Jumping"), getImage("creatures/OhNoes"), getImage("creatures/BabyWinston"), getImage("creatures/Winston"), customIcon()
];

if (userAccount.icon === iconArray.length - 1) {
    var customIconUsed = true;
} else {
    var customIconUsed = false;
}

var versionHistory = '1.2.22';

var drawIcon = function(x, y, w) {
    if (customIconUsed) {
        translate(x, y);
        scale(w/100, w/100);
        customIcon(0, 0);
        scale(100/w, 100/w);
        translate(-x, -y);
    } else {
        image(iconArray[userAccount.icon], x, y, w, w);
    }
    noFill();
    stroke(0);
    rect(x, y, w, w);
};

var printSave = function() {
    println('Copy and paste this over the previous data in "userAccount" near the top of the code, then click "Save" or "Save as Spin-off" in the bottom left corner:');
    println('name: "' + userAccount.name + '",');
    println("nameArray: [\"" + userAccount.nameArray.join("\",\"") + "\"],");
    println('password: [' + userAccount.password + '],'); 
    println('icon: ' + userAccount.icon + ',');
    println('favorites: [' + userAccount.favorites + '],');
    println('history: [' + userAccount.history + '],');
    println('setup: true,');
    println('//End//');
};

var loggedIn = false;
var currentScreen = 'home';
var searchString = [];
var searchStringDisplay = '';
var psearchString = [];
var blurNum = 0;
var overButton = false;
var endClick = false;
var endPress = false;
var searchTextSize = 20;
var searchMode = 1;
var search = false;
var outputArray = [];
var noMatch = false;
var foundMatch = false;
var results = '';
var sS = 0;
var rTS = [];
var rTN = [];
var rTA = [];
var resultPage = false;
var rPE = 0;
var NTS = 0;
var ATS = 0;
var searchHelp = false;
var submitStringDisplay = ['', '', 'https://khanacademy.org/cs/', '', '', ''];
var submitString = [[], [], ['h', 't', 't', 'p', 's', ':', '/', '/', 'k', 'h', 'a', 'n', 'a', 'c', 'a', 'd', 'e', 'm', 'y', '.', 'o', 'r', 'g', '/', 'c', 's', '/'], [], [], []];
var psubmitString = submitString;
var tS = 0;
var STS = [0, 0, 0];
var TTS = [0, 0];
var tS2 = 0;
var npString = [[], []];
var pnpString = npString;
var npStringDisplay = ['', ''];
var alert1 = 0;
var resave = false;
var LITS = 30;
var checkPassword = false;
var welcomeTimer = 0;
var favoritesArray = [];
var pfavoritesArray = favoritesArray;
var searchHistory = false;
var passwordString = '';
var alert2 = false;
var alertAll = false;
var entryNum = -10;
var textDiminish = 0;
var entryNumArray = [];
var alert3 = false;
var alert4 = false;
var newAccount = false;

//Characters for typing
var charArray = {
    48: '0',
    49: '1',
    50: '2',
    51: '3',
    52: '4',
    53: '5',
    54: '6',
    55: '7',
    56: '8',
    57: '9',
    
    65: 'A',
    66: 'B',
    67: 'C',
    68: 'D',
    69: 'E',
    70: 'F',
    71: 'G',
    72: 'H',
    73: 'I',
    74: 'J',
    75: 'K',
    76: 'L',
    77: 'M',
    78: 'N',
    79: 'O',
    80: 'P',
    81: 'Q',
    82: 'R',
    83: 'S',
    84: 'T',
    85: 'U',
    86: 'V',
    87: 'W',
    88: 'X',
    89: 'Y',
    90: 'Z',
    
    97: 'a',
    98: 'b',
    99: 'c',
    100: 'd',
    101: 'e',
    102: 'f',
    103: 'g',
    104: 'h',
    105: 'i',
    106: 'j',
    107: 'k',
    108: 'l',
    109: 'm',
    110: 'n',
    111: 'o',
    112: 'p',
    113: 'q',
    114: 'r',
    115: 's',
    116: 't',
    117: 'u',
    118: 'v', 
    119: 'w',
    120: 'x',
    121: 'y',
    122: 'z',
    
    33: '!',
    64: '@',
    35: '#',
    36: '$',
    37: '%',
    94: '^',
    38: '&',
    42: '*',
    40: '(',
    41: ')',
    
    96: '`',
    126: '~',
    45: '-',
    95: '_',
    61: '=',
    43: '+',
    91: '[',
    123: '{',
    93: ']',
    125: '}',
    92: '\\',
    124: '|',
    59: ';',
    58: ':',
    39: "'",
    34: '"',
    44: ',',
    60: '<',
    46: '.',
    62: '>',
    47: '/',
    63: '?',
    32: ' ',
};

var keySelect = 0;

var keys = [];
var keyReleased = function(){keys[keyCode] = false;};
var keyPressed = function(){keys[keyCode] = true;};

//KEY TYPED
//Allows user to edit strings
var keyTyped = function() {
    keySelect = key + 0;
    
    if (currentScreen === 'search' && !searchHelp) {
        if (keys[8] || keys[127]) {
            searchString = shorten(psearchString);
        } else {
            if (charArray[keySelect] !== undefined) {
                searchString[searchString.length] = charArray[keySelect];
            }
        }
        psearchString = searchString;
        searchStringDisplay = '';
        for (var i = 0; i < searchString.length; i += 1) {
            searchStringDisplay += searchString[i];
        }
    }
    textSize(14);
    if (currentScreen === 'submit' && ((tS < 2 && psubmitString[tS].length < 30) || tS === 2 || (tS > 2 && textWidth(submitStringDisplay[tS]) < 210))) {
        if (keys[8] || keys[127]) {
            submitString[tS] = shorten(psubmitString[tS]);
        } else {
            if (charArray[keySelect] !== undefined) {
                submitString[tS][submitString[tS].length] = charArray[keySelect];
            }
        }
        psubmitString[tS] = submitString[tS];
        submitStringDisplay[tS] = '';
        for (var i = 0; i < submitString[tS].length; i += 1) {
            submitStringDisplay[tS] += submitString[tS][i];
        }
        if ((keys[9] || keys[DOWN]) && tS < 5) {
            tS += 1;
        }
        if (keys[UP] && tS > 0) {
            tS -= 1;
        }
    } else {
        if (currentScreen === 'submit') {
            if (keys[8] || keys[127]) {
                submitString[tS] = shorten(psubmitString[tS]);
            }
            psubmitString[tS] = submitString[tS];
            submitStringDisplay[tS] = '';
            for (var i = 0; i < submitString[tS].length; i += 1) {
                submitStringDisplay[tS] += submitString[tS][i];
            }
        }
    }
    
    
    if (currentScreen === 'accountSettings' || currentScreen === 'login') {
        if (tS2 === 0 && currentScreen === 'accountSettings') {
            if (keys[8] || keys[127]) {
                npString[0] = shorten(pnpString[0]);
            } else {
                if (charArray[keySelect] !== undefined) {
                    npString[0][npString[0].length] = charArray[keySelect];
                }
            }
            pnpString[0] = npString[0];
            npStringDisplay[0] = '';
            for (var i = 0; i < npString[0].length; i += 1) {
                npStringDisplay[0] += npString[0][i];
            }
        }
        if (tS2 === 1 || currentScreen === 'login') {
            if (keys[8] || keys[127]) {
                npString[1] = shorten(pnpString[1]);
            } else {
                if (charArray[keySelect] !== undefined) {
                    npString[1][npString[1].length] = keySelect;
                }
            }
            pnpString[1] = npString[1];
            npStringDisplay[1] = '';
            for (var i = 0; i < npString[1].length; i += 1) {
                npStringDisplay[1] += '*';
            }
        }
    }
};

//CHECK REPEAT
//Check for repeats when listing results
var checkRepeat = function(entry) {
    var repeat = false;
    for (var r = 0; r < outputArray.length; r += 1) {
        if (entry === outputArray[r]) {
            repeat = true;
        }
    }
    return repeat;
};


//SEARCH ENTRIES
//Returns results for Search Engine
var searchEntries = function(input, mode) {
    outputArray = [];
    noMatch = true;
    entryNumArray = [];
    for (var e = 0; e < entries.length; e += 1) {
        foundMatch = true; 
        var n = null;
        if (searchMode === 1) {
            n = 0;
        } else if (searchMode === 2) {
            n = 1;
        } else if (searchMode === 4) {
            n = 2;
        }
        if (searchMode === 3) {
            foundMatch = false;
            for (var i = 0; i < 3; i ++) {
                if (entries[e][i + 3].indexOf(input.join("")) !== -1) {
                    foundMatch = true;
                }
            }
        } else if (entries[e][n].indexOf(input.join("")) === -1) {
            foundMatch = false;
        }
        if (foundMatch && !checkRepeat(entries[e])) {
            outputArray[outputArray.length] = entries[e];
            entryNumArray[entryNumArray.length] = e;
            noMatch = false;
        }
    }
    if (noMatch) {
        return 'No results found';
    } else {
        return outputArray;
    }
};

var popupWindow = function() {
    fill(255);
    stroke(0);
    rect(50, 50, 300, 300, 20);
    fill(255, 0, 0);
    textSize(30);
    textAlign(CENTER, CENTER);
    text('ATTENTION', 200, 70);
    fill(0);
    textSize(14);
    text('(Click to resume)', 200, 330);
    textAlign(LEFT, TOP);
    text('To make this search engine more effective\nand useful, it must first acquire a significant\ndatabase of community-made programs.', 60, 90);
    fill(0, 0, 255);
    text('If you would like one of your programs to be\npart of this search engine, and viewable to\nanyone who enters this program, please\nsubmit it today.', 60, 138);
    fill(0);
    text('Anyone can enter a program into this search\nengine. Submit as many of your programs as\nyou can to make this the best resource for\nfinding programs on Khan Academy.\nThank you! Click Submit for more info.', 60, 200);
    if (mouseIsPressed && endClick) {
        popup = false;
        endClick = false;
    }
};

var searchHelpWindow = function() {
    fill(255);
    stroke(0);
    rect(50, 50, 300, 300, 20);
    fill(0);
    textSize(30);
    textAlign(CENTER, CENTER);
    text('HELP', 200, 70);
    fill(0);
    textSize(14);
    text('(Click to resume)', 200, 330);
    textAlign(LEFT, TOP);
    text('Type in your search into the search bar, then\npress ENTER or click the icon to view the\nresults.\n     -searches must be in the correct category.\n     -searches are case sensitive.\n     -cannot search segments starting in the\nmiddle. For example, searching "Min" will\nresult in "Mini Putt", but searching "Putt", will\nyield no results.\n     -putting nothing in the searchbar will yield\nthe entire database.\n     -results are ordered from oldest to most\nrecent.', 60, 90);
    if (mouseIsPressed && endClick) {
        searchHelp = false;
        endClick = false;
    }
};

var draw = function() {
    strokeWeight(2);
    background(255);
    
    
    switch(currentScreen) {
        //HOME SCREEN
        case 'home':
            translate(blurNum, blurNum);
            textAlign(CENTER, CENTER);
            fill(0);
            textSize(40);
            text('K.A.S.E.', 200, 90);
            stroke(0);
            noFill();
            rect(110, 60, 180, 60, 10);
            strokeWeight(1);
            for (var i = 0; i < 20; i += 1) {
                stroke(255, 255, 255, i * 12);
                line(50, 90 + i, 350, 90 + i);
            }
            strokeWeight(2);
            textSize(15);
            noStroke();
            fill(255);
            rect(100, 110, 200, 20);
            fill(100);
            text('Khan Academy Search Engine', 200, 120);
            textSize(10);
            textAlign(LEFT, CENTER);
            text('Version ' + versionHistory, 12, 385);
            textAlign(CENTER, CENTER);
            text('by Matt', 370, 385);
            
            fill(0);
            textSize(14);
            text(entries.length + ' total entries', 200, 360);
            textSize(20);
            noFill();
            stroke(0);
            rect(150, 145, 100, 40, 10);
            rect(150, 195, 100, 40, 10);
            rect(150, 245, 100, 40, 10);
            rect(150, 295, 100, 40, 10);
            fill(0);
            text('Search', 200, 165);
            text('Submit', 200, 215);
            if (loggedIn) {
                text('Account', 200, 265);
            } else {
                text('Login', 200, 265);
            }
            text('About', 200, 315);
            
            overButton = false;
            if (blurNum > 0) {
                filter(BLUR, blurNum);
            }
            translate(-blurNum, -blurNum);
            
            if (mouseX > 150 && mouseX < 250 && mouseY > 145 && mouseY < 185 && !popup) {
                stroke(150, 0, 0, 200);
                fill(255, 255, 255, 200);
                rect(150, 145, 100, 40, 10);
                fill(150, 0, 0, 200);
                text('Search', 200, 165);
                overButton = true;
                if (mouseIsPressed && endClick) {
                    currentScreen = 'search';
                    endClick = false;
                }
            }
            if (mouseX > 150 && mouseX < 250 && mouseY > 195 && mouseY < 235 && !popup) {
                stroke(150, 0, 0, 200);
                fill(255, 255, 255, 200);
                rect(150, 195, 100, 40, 10);
                fill(150, 0, 0, 200);
                text('Submit', 200, 215);
                overButton = true;
                if (mouseIsPressed && endClick) {
                    currentScreen = 'submit';
                    if (loggedIn) {
                        submitString[1] = userAccount.nameArray;
                        submitStringDisplay[1] = userAccount.name;
                    }
                    endClick = false;
                }
            }
            if (loggedIn) {
                if (mouseX > 150 && mouseX < 250 && mouseY > 245 && mouseY < 285 && !popup) {
                    stroke(150, 0, 0, 200);
                    fill(255, 255, 255, 200);
                    rect(150, 245, 100, 40, 10);
                    fill(150, 0, 0, 150);
                    text('Account', 200, 265);
                    overButton = true;
                    if (mouseIsPressed && endClick) {
                        currentScreen = 'myAccount';
                        endClick = false;
                    }
                }
            } else {
                if (mouseX > 150 && mouseX < 250 && mouseY > 245 && mouseY < 285 && !popup) {
                    stroke(150, 0, 0, 200);
                    fill(255, 255, 255, 200);
                    rect(150, 245, 100, 40, 10);
                    fill(150, 0, 0, 150);
                    text('Login', 200, 265);
                    overButton = true;
                    if (mouseIsPressed && endClick) {
                        currentScreen = 'login';
                        endClick = false;
                    }
                }
            }
            if (mouseX > 150 && mouseX < 250 && mouseY > 295 && mouseY < 335 && !popup) {
                stroke(150, 0, 0, 200);
                fill(255, 255, 255, 200);
                rect(150, 295, 100, 40, 10);
                fill(150, 0, 0, 150);
                text('About', 200, 315);
                overButton = true;
                if (mouseIsPressed && endClick) {
                    currentScreen = 'about';
                    endClick = false;
                }
            }
            
            if (overButton && !popup) {
                if (blurNum < menuBlur) {
                    blurNum += menuBlur/3;
                }
            } else {
                if (blurNum > 0) {
                    blurNum -= menuBlur/3;
                }
            }
            
            if (popup && frameCount > 40) {
                popupWindow();
            }
            
            searchStringDisplay = '';
            searchString = [];
            
            break;
        
        
        //SEARCH
        case 'search':
            //DISPLAY PAGE
            if (resultPage) {
                strokeWeight(1);
                stroke(0);
                line(0, 80, 400, 80);
                fill(0);
                textAlign(CENTER, CENTER);
            translate(0, 250);
            translate(200, 95);
            scale(0.8, 0.8);
            translate(-200, -95);
            textSize(40);
            text('K.A.S.E.', 200, 90);
            noFill();
            rect(110, 60, 180, 60, 10);
            strokeWeight(1);
            for (var i = 0; i < 20; i += 1) {
                stroke(255, 255, 255, i * 12);
                line(50, 90 + i, 350, 90 + i);
            }
            strokeWeight(2);
            textSize(15);
            noStroke();
            fill(255);
            rect(100, 110, 200, 20);
            fill(100);
            text('Khan Academy Search Engine', 200, 120);
            translate(200, 95);
            scale(10/8, 10/8);
            translate(-200, -95);
            translate(0, -250);
            fill(0);
                textSize(30 - NTS);
                if (textWidth(rPE[0]) > 350) {
                    NTS += 1;
                }
                text(rPE[0], 200, 160);
                
                textSize(20 - ATS);
                if (textWidth('by ' + rPE[1]) > 350) {
                    ATS += 1;
                }
                text('by ' + rPE[1], 200, 190);
                textSize(10);
                text(rPE[2], 200, 90);
                textAlign(LEFT, CENTER);
                textSize(12);
                text(entryNum, 15, 110);
                text('Tags:\n' + rPE[3] + ',\n' + rPE[4] + ',\n' + rPE[5], 15, 270);
                strokeWeight(2);
                stroke(0);
                textAlign(CENTER, CENTER);
                if (mouseX > 10 && mouseX < 60 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        resultPage = false;
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(10, 360, 50, 30, 10);
                fill(0);
                textSize(12);
                text('BACK', 35, 376);
                if (mouseX > 340 && mouseX < 390 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        println('Copy and paste the link your browser:');
                        println(rPE[2]);
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(340, 360, 50, 30, 10);
                fill(0);
                text('LINK', 365, 376);
                
                var inFavorites = false;
                for (var i = 0; i < userAccount.favorites.length; i += 1) {
                    if (entryNum === userAccount.favorites[i]) {
                        inFavorites = true;
                    }
                }
                stroke(100);
                textAlign(CENTER, CENTER);
                var pfavorites = [];
                if (loggedIn) {
                if (inFavorites) {
                    //Delete from favorites
                    if (mouseX >= 120 && mouseX <= 280 && mouseY >= 220 && mouseY <= 240) {
                        if (mouseIsPressed && endClick) {
                            pfavorites = [];
                            for (var i = 0; i < userAccount.favorites.length; i += 1) {
                                if (userAccount.favorites[i] !== entryNum) {
                                    pfavorites[pfavorites.length] = userAccount.favorites[i];
                                }
                            }
                            userAccount.favorites = pfavorites;
                            textDiminish = 255;
                            endClick = false;
                        }
                        fill(230);
                    } else {
                        noFill();
                    }
                    textSize(14);
                    rect(120, 220, 140, 20);
                    fill(230, 25, 25);
                    rect(260, 220, 20, 20);
                    fill(0);
                    text('Delete from Favorites', 190, 230);
                    textSize(18);
                    text('-', 270, 229);
                } else {
                    //Add to favorites
                    if (mouseX >= 135 && mouseX <= 265 && mouseY >= 220 && mouseY <= 240) {
                        if (mouseIsPressed && endClick) {
                            userAccount.favorites[userAccount.favorites.length] = entryNum;
                            endClick = false;
                            textDiminish = 255;
                        }
                        fill(230);
                    } else {
                        noFill();
                    }
                    textSize(14);
                    rect(135, 220, 110, 20);
                    fill(25, 230, 25);
                    rect(245, 220, 20, 20);
                    fill(0);
                    text('Add to Favorites', 190, 230);
                    textSize(18);
                    text('+', 255, 229);

                }
                if (textDiminish > 0) {
                    textSize(11);
                    fill(0, 0, 0, textDiminish);
                    text('Remember to save changes', 200, 250);
                    textDiminish -= 2.55;
                }
                }
                if (mouseY < 80 && mouseIsPressed && endClick) {
                    resultPage = false;
                    endClick = false;
                }
            } else {
                NTS = 0;
                ATS = 0;
            var trans = false;
            //DISPLAY RESULTS
            if (results === 'No results found') {
                textAlign(CENTER, CENTER);
                textSize(20);
                text(results, 200, 200);
            } else {
                strokeWeight(1);
                for (var i = 0; i < results.length; i += 1) {
                    noFill();
                    stroke(0);
                    if (mouseY > 70 && mouseX > 20 && mouseX < 380 && mouseY> 70 + i * 33 + sS && mouseY <= 70 + i * 33 + 33 + sS && !searchHelp && !searchHistory) {
                        rect(20, 70 + i * 33 + sS, 360, 43);
                        textSize(10);
                        textAlign(LEFT, CENTER);
                        text(results[i][2], 25, 70 + i * 33 + 35 + sS);
                    } else {
                        rect(20, 70 + i * 33 + sS, 360, 33);
                    }
                    fill(0);
                    if (searchHistory) {
                        textSize(20 - rTS[i]);
                        rTN[i] = textWidth(results[i]);
                        rTA[i] = 0;
                        if (rTN[i] > 350) {
                            rTS[i] += 1;
                        }
                    } else {
                        textSize(20 - rTS[i]);
                        rTN[i] = textWidth(results[i][0]);
                        
                        textSize(14 - rTS[i]);
                        rTA[i] = textWidth('by ' + results[i][1]);
                        if (rTN[i] + rTA[i] > 350) {
                        rTS[i] += 1;
                    }
                    }
                    
                    //Name
                    textAlign(LEFT, CENTER);
                    textSize(20 - rTS[i]);
                    if (searchHistory) {
                        text(results[i], 25, 90 + i * 33 + sS);
                    } else {
                        text(results[i][0], 25, 90 + i * 33 + sS);
                        //Author
                        if (results[i][1] === 'Matt') {
                            fill(255, 0, 0);
                        } else {
                            fill(100);
                        }
                        textSize(14 - rTS[i]);
                        textAlign(RIGHT, CENTER);
                        text('by ' + results[i][1], 375,  90 + i * 33 + sS);
                    }
                    fill(0);
                    
                    if (mouseY > 70 && mouseX > 20 && mouseX < 380 && mouseY> 70 + i * 33 + sS && mouseY <= 70 + i * 33 + 33 + sS && !searchHelp) {
                        if (mouseIsPressed && endClick) {
                            if (searchHistory) {
                                search = true;
                                for (var h = 0; h < results[i].length; h += 1) {
                                    searchString += results[i][h];
                                }
                                searchStringDisplay = results[i];
                                searchHistory = false;
                            } else {
                                resultPage = true;
                                rPE = results[i];
                                entryNum = entryNumArray[i];
                                println('Copy and paste the link to your browser:');
                                println(rPE[2]);
                            }
                            endClick = false;
                        }
                        if (!searchHistory) {
                            trans = true;
                            translate(0, 10);
                        }
                    }
                }
                strokeWeight(2);
            }
            
            if (trans) {
                translate(0, -10);
            }
            
            noStroke();
            fill(255);
            rect(0, 0, 400, 70);
            
            //scroll bar
            if (results.length >= 10 && results !== 'No results found') {
                for (var i = 0; i < scrollSpeed.length; i += 1) {
                    if (mouseX < 380) {
                        stroke(200);
                    } else {
                        if (mouseY < 98 - i * 8) {
                            stroke(150, 0, 0);
                            if (sS < 0) {
                            sS += scrollSpeed[i];
                            } else {
                                sS = 0;
                            }
                        } else {
                            stroke(150);
                        }
                    }
                    line(386, 98 - i * 8, 390, 92 - i * 8);
                    line(390, 92 - i * 8, 394, 98 - i * 8);
                    if (mouseX < 380) {
                        stroke(200);
                    } else {
                        if (mouseY > 372 + i * 8) {
                            stroke(150, 0, 0);
                            if (sS > -results.length * 33 + 300) {
                            sS -= scrollSpeed[i];
                            } else {
                                sS = -results.length * 33 + 300;
                            }
                        } else {
                            stroke(150);
                        }
                    }
                    line(386, 372 + i * 8, 390, 378 + i * 8);
                    line(390, 378 + i * 8, 394, 372 + i * 8);
                }
            }
            }
            
            
            
            textAlign(CENTER, CENTER);
            textSize(11);
            
            if (mouseX >= 15 && mouseX < 65 && mouseY >= 20 && mouseY <= 35 && searchMode !== 1) {
                stroke(255, 0, 0);
                if (mouseIsPressed && endClick) {
                    searchMode = 1;
                    endClick = false;
                }
            } else {
                stroke(0);
            }
            
            if (searchMode === 1) {
                fill(220);
            } else {
                fill(255);
            }
            rect(15, 20, 50, 20, 5);
            fill(0);
            text('Name', 40, 28);
            if (searchMode !== 1) {
                line(15, 35, 65, 35);
            }
            
            if (mouseX >= 65 && mouseX < 115 && mouseY >= 20 && mouseY <= 35  && searchMode !== 2) {
                stroke(255, 0, 0);
                if (mouseIsPressed && endClick) {
                    searchMode = 2;
                    endClick = false;
                }
            } else {
                stroke(0);
            }
            
            if (searchMode === 2) {
                fill(220);
            } else {
                fill(255);
            }
            rect(65, 20, 50, 20, 5);
            fill(0);
            text('Author', 90, 28);
            if (searchMode !== 2) {
                line(65, 35, 115, 35);
            }
            
            if (mouseX >= 115 && mouseX < 165 && mouseY >= 20 && mouseY <= 35  && searchMode !== 3) {
                stroke(255, 0, 0);
                if (mouseIsPressed && endClick) {
                    searchMode = 3;
                    endClick = false;
                }
            } else {
                stroke(0);
            }
            
            if (searchMode === 3) {
                fill(0);
                textSize(9);
                text('* recommended to search in lower case', 300, 28);
                textSize(11);
                fill(220);
            } else {
                fill(255);
            }
            rect(115, 20, 50, 20, 5);
            fill(0);
            text('Tags', 140, 28);
            if (searchMode !== 3) {
                line(115, 35, 165, 35);
            }
            
            
            if (mouseX >= 165 && mouseX < 215 && mouseY >= 20 && mouseY <= 35  && searchMode !== 4) {
                stroke(255, 0, 0);
                if (mouseIsPressed && endClick) {
                    searchMode = 4;
                    endClick = false;
                }
            } else {
                stroke(0);
            }
            
            if (searchMode === 4) {
                fill(220);
            } else {
                fill(255);
            }
            rect(165, 20, 50, 20, 5);
            fill(0);
            text('Link', 190, 28);
            if (searchMode !== 4) {
                line(165, 35, 215, 35);
            }
            
            stroke(0);
            line(215, 35, 385, 35);
            
            fill(255);
            noStroke();
            rect(0, 36, 400, 34);
            stroke(0);
            noFill();
            arc(15, -15, 100, 100, 90, 130);
            arc(385, -15, 100, 100, 40, 90);
            stroke(0);
            noFill();
            rect(15, 40, 370, 30, 10);
            
            
            //LEFT / RIGHT
            if (keys[LEFT] && searchMode > 1 && endPress) {
                searchMode -= 1;
                endPress = false;
            }
            if (keys[RIGHT] && searchMode < 4 && endPress) {
                searchMode += 1;
                endPress = false;
            }
            
            //search button
            if (mouseX >= 359 && mouseX <= 377 && mouseY >= 45 && mouseY <= 63) {
                stroke(120);
                if (mouseIsPressed && endClick) {
                    endClick = false;
                    search = true;
                }
            } else {
                stroke(180);
            }
            strokeWeight(4);
            ellipse(365, 52, 13, 13);
            line(370, 57, 377, 63);
            strokeWeight(2);
            stroke(0);
            
            fill(0);
            if (loggedIn) {
                textSize(LITS - 12);
                textAlign(RIGHT, CENTER);
                drawIcon(355 - textWidth(userAccount.name), 4, 14);
                text(userAccount.name, 380, 11);
                if (mouseX >= 355 - textWidth(userAccount.name) && mouseY <= 18 && mouseIsPressed && endClick) {
                    currentScreen = 'myAccount';
                    endClick = false;
                }
            textSize(11);
                textAlign(LEFT, CENTER);
                if (results !== 'No results found') {
                    if (results.length === 1) {
                        text('1 result', 20, 10);
                    } else {
                        text(results.length + ' results', 20, 10);
                    }
                }
                textAlign(RIGHT, CENTER);
            } else {
                text('K.A.S.E.', 30, 10);
                if (results !== 'No results found') {
                    if (results.length === 1) {
                        text('1 result', 200, 10);
                    } else {
                        text(results.length + ' results', 200, 10);
                    }
                }
                text('by Matt', 370, 10);
            }
            
            textAlign(LEFT, CENTER);
            textSize(searchTextSize);
            if (textWidth(searchStringDisplay) > 330) {
                searchTextSize -= 1;
            } else {
                textSize(searchTextSize + 1);
                if (textWidth(searchStringDisplay) < 330 && searchTextSize < 20) {
                    searchTextSize += 1;
                }
                textSize(searchTextSize);
            }
            text(searchStringDisplay, 25, 55);
            if (round(frameCount/18) % 2 === 0 && !keyIsPressed) {
                line(25 + textWidth(searchStringDisplay) + 1, 45, 25 + textWidth(searchStringDisplay) + 1, 65);
            }
            
            if (keys[10]) {
                search = true;
            }
            
            if (search && !searchHelp) {
                results = searchEntries(searchString, searchMode);
                for (var i = 0; i < results.length; i += 1) {
                    rTN[i] = 0;
                    rTA[i] = 0;
                    rTS[i] = 0;
                }
                search = false;
                searchHistory = false;
                if (userAccount.history[userAccount.history.length - 1] !== searchStringDisplay) {
                    userAccount.history[userAccount.history.length] = searchStringDisplay;
                }
                sS = 0;
                
            }
            if (!resultPage) {
                if (mouseX >= 380) {
                    if (mouseY > 215 && mouseY < 235) {
                        if (mouseIsPressed && endClick) {
                            searchHelp = true;
                            endClick = false;
                        }
                        fill(150, 0, 0);
                    } else {
                    fill(150);
                    }
                } else {
                    fill(200);
                }
                textSize(20);
                textAlign(CENTER, CENTER);
                text('?', 390, 225);
            }
            
            //SEARCH HELP
            if (searchHelp) {
                searchHelpWindow();
            }
            if (!resultPage) {
                if (mouseX <= 20) {
                    if (mouseY > 215 && mouseY < 235) {
                        if (mouseIsPressed && endClick) {
                            currentScreen = 'home';
                            endClick = false;
                        }
                        fill(150, 0, 0);
                    } else {
                        fill(150);
                    }
                } else {
                    fill(200);
                }
                textSize(20);
                text('<', 10, 225);
            }
            
            fill(0);
            
            break;
        
        case 'submit':
            if (loggedIn) {
                textSize(LITS - 12);
                textAlign(RIGHT, CENTER);
                drawIcon(355 - textWidth(userAccount.name), 4, 14);
                text(userAccount.name, 380, 11);
                if (mouseX >= 355 - textWidth(userAccount.name) && mouseY <= 18 && mouseIsPressed && endClick) {
                    currentScreen = 'myAccount';
                    endClick = false;
                }
            }
            fill(0);
            textSize(30);
            textAlign(CENTER, CENTER);
            text('Submit', 200, 35);
            textAlign(LEFT, TOP);
            textSize(15);
            text('Name:', 10, 72);
            text('Author:', 10, 132);
            text('Link:', 10, 192);
            text('Tag', 10, 252);
            text('1:', 40, 252);
            text('2:', 40, 286);
            text('3:', 40, 320);
            textSize(12);
            text('*maximum 30 characters', 200, 100);
            text('*maximum 30 characters', 200, 160);
            text('must connect to a Khan Academy program', 100, 220);
            text('**', 320, 252);
            text('**', 320, 286);
            text('**', 320, 320);
            text('** Optional. Recomended you use all lower case.', 60, 350);
            noFill();
            stroke(0);
            rect(60, 70, 300, 24);
            rect(60, 130, 300, 24);
            rect(60, 190, 300, 24);
            rect(60, 250, 250, 24);
            rect(60, 284, 250, 24);
            rect(60, 318, 250, 24);
            if (mouseX >= 60 && mouseX <= 300) {
                if (mouseY >= 70 && mouseY <= 94 && mouseIsPressed && endClick) {
                    tS = 0;
                    endClick = false;
                }
                if (mouseY >= 130 && mouseY <= 154 && mouseIsPressed && endClick) {
                    tS = 1;
                    endClick = false;
                }
                if (mouseY >= 190 && mouseY <= 214 && mouseIsPressed && endClick) {
                    tS = 2;
                    endClick = false;
                }
                if (mouseX <= 250) {
                    if (mouseY >= 250 && mouseY <= 274 && mouseIsPressed && endClick) {
                        tS = 3;
                        endClick = false;
                    }
                    if (mouseY >= 284 && mouseY <= 308 && mouseIsPressed && endClick) {
                        tS = 4;
                        endClick = false;
                    }
                    if (mouseY >= 318 && mouseY <= 342 && mouseIsPressed && endClick) {
                        tS = 5;
                        endClick = false;
                    }
                }
            }
            
            fill(0);
            textAlign(LEFT, CENTER);
            var tS16 = 16;
            for (var i = 0; i < 3; i += 1) {
                if (i === 2) {
                    tS16 = 12;
                }
                textSize(tS16 - STS[i]);
                if (textWidth(submitStringDisplay[i]) > 290) {
                    STS[i] += 1;
                } else {
                    textSize(tS16 - (STS[i] - 1));
                    if (textWidth(submitStringDisplay[i]) < 290 && STS[i] > 0) {
                        STS[i] -= 1;
                    }
                    textSize(tS16 - STS[i]);
                }
                text(submitStringDisplay[i], 65, 82 + i * 60);
                if (round(frameCount/18) % 2 === 0 && !keyIsPressed && tS === i) {
                    line(65 + textWidth(submitStringDisplay[i]) + 2, 76 + i * 60, 65 + textWidth(submitStringDisplay[i]) + 2, 88 + i * 60);
                }
            }
            
            textSize(16);
            fill(0);
            for (var i = 0; i < 3; i += 1) {
                text(submitStringDisplay[i + 3], 65, 262 + i * 34);
                if (round(frameCount/18) % 2 === 0 && !keyIsPressed && tS === i + 3) {
                    line(65 + textWidth(submitStringDisplay[i + 3]) + 2, 256 + i * 34, 65 + textWidth(submitStringDisplay[i + 3]) + 2, 268 + i * 34);
                }
            }
            
            
            
            
            textAlign(CENTER, CENTER);
            if (mouseX > 10 && mouseX < 60 && mouseY > 360 && mouseY < 390) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    currentScreen = 'home';
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(10, 360, 50, 30, 10);
            fill(0);
            textSize(12);
            text('BACK', 35, 376);
                if (mouseX > 330 && mouseX < 390 && mouseY > 360 && mouseY < 390) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    println('Copy and paste this into the comments, then I can enter it into the database:');
                    println("['" + submitStringDisplay[0] + "', '" + submitStringDisplay[1] + "', '" + submitStringDisplay[2] + "', '" + submitStringDisplay[3] + "', '" + submitStringDisplay[4] + "', '" + submitStringDisplay[5] + "'],");
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(330, 360, 60, 30, 10);
            fill(0);
            text('SUBMIT', 360, 376);
            
            break;
            
        //Login
        case 'login':
            
            if (userAccount.setup) {
                if (!loggedIn) {
                fill(0);
                textSize(20);
                textAlign(CENTER, CENTER);
                text('Enter Password:', 200, 200);
                textSize(LITS);
                textAlign(LEFT, CENTER);
                drawIcon(200 - (textWidth(userAccount.name) + 60)/2, 100, 50);
                text(userAccount.name, 260 - (textWidth(userAccount.name) + 60)/2, 125);
                
                textSize(18 - TTS[1]);
                if (textWidth(npStringDisplay[1]) > 290) {
                    TTS[1] += 1;
                } else {
                    textSize(18 - (TTS[1] - 1));
                    if (textWidth(npStringDisplay[1]) < 290 && TTS[1] > 0) {
                        TTS[1] -= 1;
                    }
                    textSize(18 - TTS[1]);
                }
                text(npStringDisplay[1], 55, 227);
                if (round(frameCount/18) % 2 === 0 && !keyIsPressed) {
                    line(55 + textWidth(npStringDisplay[1]) + 2, 219, 55 + textWidth(npStringDisplay[1]) + 2, 234);
                }
                
                if (alert1 > 0) {
                    alert1 -= 1;
                    if (floor(alert1/6) % 2 === 0) {
                        fill(255, 0, 0);
                    } else {
                        noFill();
                    }
                } else {
                    noFill();
                    alert1 = 0;
                }
                rect(50, 215, 300, 24);
                
                textAlign(CENTER, CENTER);

                if (mouseX > 10 && mouseX < 60 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        currentScreen = 'home';
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(10, 360, 50, 30, 10);
                fill(0);
                textSize(12);
                text('BACK', 35, 376);
                if (mouseX > 330 && mouseX < 390 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        checkPassword = true;
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(330, 360, 60, 30, 10);
                fill(0);
                text('LOGIN', 360, 376);
                
                if (keys[10]) {
                    checkPassword = true;
                }
                
                var CP = true;
                if (checkPassword) {
                    for (var i = 0; i < userAccount.password.length; i += 1) {
                        if (npString[1][i] !== userAccount.password[i]) {
                            CP = false;
                        }
                    }
                    if (CP) {
                        currentScreen = 'welcome';
                        loggedIn = true;
                    } else {
                        alert1 = 30;
                    }
                    checkPassword = false;
                }
                }
                
            } else {
                //Create Account
                fill(0);
                stroke(0);
                textSize(20);
                textAlign(CENTER, CENTER);
                text('There is no account set up for this program.\nWould you like to create one?', 200, 200);
                if (mouseX > 10 && mouseX < 60 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        currentScreen = 'home';
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(10, 360, 50, 30, 10);
                fill(0);
                textSize(12);
                text('BACK', 35, 376);
                    if (mouseX > 330 && mouseX < 390 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        currentScreen = 'accountSettings';
                        newAccount = true;
                        resave = false;
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(330, 360, 60, 30, 10);
                fill(0);
                text('CREATE', 360, 376);
            }
            
            break;
            
            
            //Account SETTINGS
        case 'accountSettings':
            fill(0);
            textAlign(CENTER, CENTER);
            textSize(26);
            text('Account Settings', 200, 35);
            textSize(15);
            textAlign(LEFT, CENTER);
            text('*Do not use KA account password!', 60, 320);
            text('**You can customize the last icon under\n   the customIcon() function.', 60, 350);
            text('Username:', 11, 85);
            text('Password:', 11, 125);
            text('Icon:', 11, 170);
            textSize(12);
            text('Password must be 6 or more characters.', 90, 145);
            for (var i = 0; i < 2; i += 1) {
                textSize(15 - TTS[i]);
                if (textWidth(npStringDisplay[i]) > 290) {
                    TTS[i] += 1;
                } else {
                    textSize(15 - (TTS[i] - 1));
                    if (textWidth(npStringDisplay[i]) < 290 && TTS[i] > 0) {
                        TTS[i] -= 1;
                    }
                    textSize(15 - TTS[i]);
                }
                text(npStringDisplay[i], 95, 85 + i * 40);
                if (round(frameCount/18) % 2 === 0 && !keyIsPressed && tS2 === i) {
                    line(95 + textWidth(npStringDisplay[i]) + 2, 77 + i * 40, 95 + textWidth(npStringDisplay[i]) + 2, 92 + i * 40);
                }
            }
            
            if (mouseX >= 90 && mouseX <= 390) {
                if (mouseY >= 73 && mouseY <= 97 && mouseIsPressed && endClick) {
                    tS2 = 0;
                    endClick = false;
                }
                if (mouseY >= 113 && mouseY <= 137 && mouseIsPressed && endClick) {
                    tS2 = 1;
                    endClick = false;
                }
            }
            
            stroke(0);
            if (alert1 > 0) {
                alert1 -= 1;
                if (floor(alert1/6) % 2 === 0) {
                    fill(255, 0, 0);
                } else {
                    noFill();
                }
            } else {
                noFill();
                alert1 = 0;
            }
            rect(90, 73, 300, 24);
            rect(90, 113, 300, 24);
            
            var iconPick = 0;
            for (var i = 0; i < iconArray.length; i += 1) {
                if (i < iconArray.length - 1) {
                    iconPick = iconArray[i];
                    image(iconPick, 60 + (i % 8) * 40, 160 + floor(i/8) * 40, 20, 20);
                } else {
                    translate(60 + (i % 8) * 40, 160 + floor(i/8) * 40);
                    scale(0.2, 0.2);
                    customIcon(0, 0);
                    scale(5, 5);
                    translate(-60 - (i % 8) * 40, -160 - floor(i/8) * 40);
                }
                noFill();
                if (userAccount.icon !== i) {
                    if (mouseX >= 60 + (i % 8) * 40 && mouseX <= 60 + (i % 8) * 40 + 20 && mouseY >= 160 + floor(i/8) * 40 && mouseY <= 160 + floor(i/8) * 40 + 20) {
                        stroke(0, 255, 0);
                        if (mouseIsPressed && endClick) {
                            userAccount.icon = i;
                            endClick = false;
                        }
                    } else {
                        stroke(0);
                    }
                } else {
                    stroke(255, 0, 0);
                }
                rect(60 + (i % 8) * 40, 160 + floor(i/8) * 40, 20, 20);
            }
            stroke(0);
            if (mouseX > 10 && mouseX < 60 && mouseY > 360 && mouseY < 390) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    if (userAccount.setup && (loggedIn || newAccount)) {
                        currentScreen = 'myAccount';
                        loggedIn = true;
                    } else {
                        currentScreen = 'login';
                    }
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(10, 360, 50, 30, 10);
            fill(0);
            textSize(12);
            text('BACK', 35, 376);
            if (resave && userAccount.setup) {
                if (mouseX > 270 && mouseX < 330 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        if (npStringDisplay[0].length > 0 && npStringDisplay[1].length >= 6) {
                            userAccount.name = npStringDisplay[0];
                            userAccount.password = npString[1];
                            userAccount.nameArray = npString[0];
                            printSave();
                        } else {
                            alert1 = 30;
                        }
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(270, 360, 60, 30, 10);
                fill(0);
                text('RESAVE', 300, 376);
                if (mouseX > 340 && mouseX < 390 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        loggedIn = true;
                        if (welcomeTimer <= 40) {
                            currentScreen = 'welcome';
                        } else {
                            currentScreen = 'myAccount';
                        }
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(340, 360, 50, 30, 10);
                fill(0);
                textSize(12);
                text('NEXT', 365, 376);
            } else {
                if (mouseX > 330 && mouseX < 390 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        if (npStringDisplay[0].length > 0 && npStringDisplay[1].length >= 6) {
                            userAccount.name = npStringDisplay[0];
                            userAccount.password = npString[1];
                            userAccount.nameArray = npString[0];
                            printSave();
                        } else {
                            alert1 = 30;
                        }
                        resave = true;
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(330, 360, 60, 30, 10);
                fill(0);
                text('SAVE', 360, 376);
            }
            if ((keys[9] || keys[DOWN]) && tS2 === 0) {
                tS2 = 1;
            }
            if (keys[UP] && tS2 === 1) {
                tS2 = 0;
            }
            
            break;
        
        
        case 'welcome':
            translate(0, welcomeTimer - 20);
            textAlign(CENTER, CENTER);
            fill(0);
            text('Welcome', 200, 170);
            textSize(LITS);
            textAlign(LEFT, CENTER);
            drawIcon(200 - (textWidth(userAccount.name) + 60)/2, 200, 50);
            text(userAccount.name, 260 - (textWidth(userAccount.name) + 60)/2, 225);
            translate(0, -welcomeTimer + 20);
            fill(255, 255, 255, welcomeTimer * 5);
            noStroke();
            rect(0, 0, 400, 400);
            
            welcomeTimer += 1;
            if (welcomeTimer > 40) {
                currentScreen = 'myAccount';
            }
            
            break;
            
            
        case 'myAccount':
            
            fill(0);
            textSize(LITS - 8);
            drawIcon(350 - textWidth(userAccount.name), 5, 20);
            textSize(LITS - 8);
            textAlign(RIGHT, CENTER);
            text(userAccount.name, 380, 15);
            line(0, 30, 400, 30);
            textAlign(CENTER, CENTER);
            textSize(11);
            text('(Tip: Make sure to save\nregularly to make full use\nof features like favorites\nor history.)', 270, 350);
            textSize(9);
            text('(Tip: If this is a spin-off make sure\nto update the database by going to\nthe original, copying the array\n"entries", and pasting it over your\nown array', 280, 145);
            
            translate(-150, -80);
            translate(200, 95);
            scale(0.4, 0.4);
            translate(-200, - 95);
            textSize(40);
            text('K.A.S.E.', 200, 90);
            stroke(0);
            noFill();
            rect(110, 60, 180, 60, 10);
            strokeWeight(1);
            for (var i = 0; i < 20; i += 1) {
                stroke(255, 255, 255, i * 12);
                line(50, 90 + i, 350, 90 + i);
            }
            strokeWeight(2);
            textSize(15);
            noStroke();
            fill(255);
            rect(100, 110, 200, 20);
            fill(100);
            text('Khan Academy Search Engine', 200, 120);
            translate(200, 95);
            scale(5/2, 5/2);
            translate(-200, - 95);
            translate(150, 80);
            
            fill(0);
            textSize(30);
            text('My Account', 200, 80);
            textSize(20);
            noFill();
            stroke(0);
            
            if (mouseX >= 70 && mouseX <= 190 && mouseY >= 125 && mouseY <= 165 && !alertAll) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    currentScreen = 'search';
                    searchString = userAccount.nameArray;
                    searchStringDisplay = userAccount.name;
                    searchMode = 2;
                    search = true;
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(70, 125, 120, 40, 10);
            
            if (mouseX >= 70 && mouseX <= 190 && mouseY >= 175 && mouseY <= 215 && !alertAll) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    if (userAccount.favorites.length > 0) {
                        currentScreen = 'search';
                        results = favoritesArray;
                        for (var i = 0; i < results.length; i += 1) {
                            rTN[i] = 0;
                            rTA[i] = 0;
                            rTS[i] = 0;
                        }
                        sS = 0;
                    } else {
                        alert2 = true;
                    }
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(70, 175, 120, 40, 10);
            
            if (mouseX >= 70 && mouseX <= 190 && mouseY >= 225 && mouseY <= 265 && !alertAll) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    currentScreen = 'search';
                    searchHistory = true;
                    results = userAccount.history;
                    for (var i = 0; i < results.length; i += 1) {
                        rTN[i] = 0;
                        rTA[i] = 0;
                        rTS[i] = 0;
                    }
                    sS = 0;
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(70, 225, 120, 40, 10);
            
            if (mouseX >= 70 && mouseX <= 190 && mouseY >= 275 && mouseY <= 315 && !alertAll) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    currentScreen = 'accountSettings';
                    npString = [userAccount.nameArray, userAccount.password];
                    npStringDisplay = [userAccount.name, passwordString];
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(70, 275, 120, 40, 10);
            
            if (mouseX >= 70 && mouseX <= 190 && mouseY >= 325 && mouseY <= 365 && !alertAll) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    currentScreen = 'home';
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(70, 325, 120, 40, 10);
            
            if (mouseX >= 210 && mouseX <= 330 && mouseY >= 275 && mouseY <= 315 && !alertAll) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    printSave();
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(210, 275, 120, 40, 10);
            
            if (mouseX >= 210 && mouseX <= 330 && mouseY >= 175 && mouseY <= 215 && !alertAll) {
                fill(170, 0, 0);
                if (mouseIsPressed && endClick) {
                    alert3 = true;
                    endClick = false;
                }
            } else {
                fill(200, 0, 0);
            }
            rect(210, 175, 120, 40, 10);
            
            if (mouseX >= 210 && mouseX <= 330 && mouseY >= 225 && mouseY <= 265 && !alertAll) {
                fill(170, 0, 0);
                if (mouseIsPressed && endClick && !alertAll) {
                    alert4 = true;
                    endClick = false;
                }
            } else {
                fill(200, 0, 0);
            }
            rect(210, 225, 120, 40, 10);
            fill(0);
            text('My Entries', 130, 145);
            text('Favorites', 130, 195);
            text('History', 130, 245);
            text('Settings', 130, 295);
            text('Exit', 130, 345);
            text('Clear', 270, 195);
            text('Clear', 270, 245);
            text('Save', 270, 295);
            
            alertAll = false;
            if (alert2) {
                alertAll = true;
                fill(255);
                stroke(100);
                rect(100, 140, 200, 120, 20);
                fill(0);
                textSize(12);
                text('There are no favorites set up for\nthis account. To add a program to\n your favorites, find it in "search"\nand click "add to favorites".\n(Click to resume)', 200, 200);
                if (mouseIsPressed && endClick) {
                    alert2 = false;
                    endClick = false;
                }
            }
            if (alert3) {
                alertAll = true;
                fill(255);
                stroke(100);
                rect(100, 140, 200, 120, 20);
                if (mouseX >= 210 && mouseX <= 290 && mouseY >= 220 && mouseY <= 250) {
                    if (mouseIsPressed && endClick) {
                        alert3 = false;
                        endClick = false;
                    }
                    fill(230);
                } else {
                    fill(255);
                }
                rect(210, 220, 80, 30, 10);
                if (mouseX >= 110 && mouseX <= 190 && mouseY >= 220 && mouseY <= 250) {
                    if (mouseIsPressed && endClick) {
                        userAccount.favorites = [];
                        alert3 = false;
                        endClick = false;
                    }
                    fill(170, 0, 0);
                } else {
                    fill(200, 0, 0);
                }
                rect(110, 220, 80, 30, 10);
                fill(0);
                textSize(12);
                text('Are you sure you want to delete\nall favorites? Remember to save\nafterward to make changes\npermanant.', 200, 180);
                textSize(20);
                text('Clear', 150, 235);
                text('Cancel', 250, 235);
                
            }
            if (alert4) {
                alertAll = true;
                fill(255);
                stroke(100);
                rect(100, 140, 200, 120, 20);
                if (mouseX >= 210 && mouseX <= 290 && mouseY >= 220 && mouseY <= 250) {
                    if (mouseIsPressed && endClick) {
                        alert4 = false;
                        endClick = false;
                    }
                    fill(230);
                } else {
                    fill(255);
                }
                rect(210, 220, 80, 30, 10);
                if (mouseX >= 110 && mouseX <= 190 && mouseY >= 220 && mouseY <= 250) {
                    if (mouseIsPressed && endClick) {
                        userAccount.history = [];
                        alert4 = false;
                        endClick = false;
                    }
                    fill(170, 0, 0);
                } else {
                    fill(200, 0, 0);
                }
                rect(110, 220, 80, 30, 10);
                fill(0);
                textSize(12);
                text('Are you sure you want to clear\nyour history? Remember to save\nafterward to make changes\npermanant.', 200, 180);
                textSize(20);
                text('Clear', 150, 235);
                text('Cancel', 250, 235);
                
            }
            
            
            
            break;
            
        case 'about':
            
            fill(0);
            textAlign(CENTER, CENTER);
            textSize(20);
            text('About', 200, 15);
            textAlign(LEFT, TOP);
            textSize(12);
            text('     K.A.S.E. is a fully functional search engine, (with other features\nas well) that relies on a local database that I will update regularly,\nbut to do so I need submissions from the community (that means\nyou). A larger database will make searches more effective and\nmore likely to yield a helpful result. So if you have any programs\nthat you would like to submit, please do so. The contribution will\nbe much appreciate.', 20, 25);
            text('You can submit any program that meets these requirements:\n   1. It is not a repeat entry.\n   2. The link connects to a Khan Academy program.\n   3. It is your program. Do not submit a program you are not the\n       author of.', 20, 130);
            text('     K.A.S.E. is the result of 4 days of relentless, obsessive\nprogramming by yours truly (11/25/13 - 11/28/13). Many hours were\nput into this and as far as I know, it is the first ever program on Khan\nAcademy to feature either a fully function search engine or user\naccounts.\n     If you know of any bugs or entries that do not meet the above\nrequirements, please report them to me in the comments section.', 20, 205);
            text('     Disclaimer: I do not own or am represented by any of the material\nthat can be found through this search engine, with the exception of\nmaterial that I post. Everything was posted with the author' + "'" + 's consent.\nThis code is an original creation by Matt.', 20, 310);
            textAlign(CENTER, CENTER);
            stroke(0);
            if (mouseX > 340 && mouseX < 390 && mouseY > 360 && mouseY < 390) {
                    fill(230);
                    if (mouseIsPressed && endClick) {
                        currentScreen = 'home';
                        endClick = false;
                    }
                } else {
                    fill(255);
                }
                rect(340, 360, 50, 30, 10);
                fill(0);
                textSize(12);
                text('BACK', 365, 376);
            
            
            break;
            
            
        default:
            background(255);
            textAlign(CENTER, CENTER);
            translate(0, 30);
            fill(0);
            textSize(40);
            text('K.A.S.E.', 200, 90);
            stroke(0);
            noFill();
            rect(110, 60, 180, 60, 10);
            strokeWeight(1);
            for (var i = 0; i < 20; i += 1) {
                stroke(255, 255, 255, i * 12);
                line(50, 90 + i, 350, 90 + i);
            }
            strokeWeight(2);
            textSize(15);
            noStroke();
            fill(255);
            rect(100, 110, 200, 20);
            fill(100);
            text('Khan Academy Search Engine', 200, 120);
            translate(0, -30);
            textSize(20);
            fill(0);
            text("ERROR:\nPage not defined.", 200, 210);
            strokeWeight(1);
            stroke(0);
            line(100, 170, 300, 170);
            strokeWeight(2);
            if (mouseX >= 140 && mouseX <= 260 && mouseY >= 255 && mouseY <= 295) {
                fill(230);
                if (mouseIsPressed && endClick) {
                    currentScreen = 'home';
                    endClick = false;
                }
            } else {
                fill(255);
            }
            rect(140, 255, 120, 40, 10);
            fill(0);
            text('Go to Home', 200, 275);
            
            break;
    }
    
    for (var i = 0; i < userAccount.favorites.length; i += 1) {
        favoritesArray[i] = entries[userAccount.favorites[i]];
    }
    pfavoritesArray = favoritesArray;
    
    passwordString = '';
    for (var i = 0; i < npString[1].length; i += 1) {
        passwordString += '*';
    }
    
    textSize(LITS);
    if (textWidth(userAccount.name) > 320) {
        LITS -= 1;
    }
    
    if (!mouseIsPressed) {
        endClick = true;
    }
    if (!keyIsPressed) {
        endPress = true;
    }
};


/*
Disclaimer: I do not own or am represented by any of the material that can be found through this search engine, with the exception of material that I post. Everything was posted with the author's consent.


70 pages of code in 4 days...
Released 12/1/2013
*/
