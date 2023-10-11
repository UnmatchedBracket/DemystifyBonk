[A Repl that encodes/decodes maps and "is" data.](https://replit.com/@UnmatchedBracket/bonkio-is-decoder)

What the map keys mean (thanks kklkkj for [this amazing file](https://github.com/kklkkj/kklee/blob/master/src/kkleeApi.nim)):
- v: The map data version.
- m: Map metadata:
  - n, a, date, mo: Map name, author, creation date, and reccomended mode.
  - rxn, rxa: Original map name and author (for edits)
  - dbid, dbv, authid: Database ID, bonk version for which the database ID is for (1 or 2), author account DBID
  - pub: Whether or not map is public
  - cr: A list of contributor's usernames.
- spawns: A list of spawn objects:
  - n: Spawn's name
  - priority: Spawn's priority
  - f, r, b, gr, ye: Booleans to indicate whether or not that team (ffa, red, blue, green, yellow) can spawn here.
  - x, y, xv, yv: Starting position and velocity
- capZones: A list of capzone objects:
  - n: Capzone's name
  - ty: Cap zone type. 1,2,3,4,5 = normal, instant [red, blue, green, yellow] win
  - i: Fixture ID to attach to
  - l: Time to capture for normal
- physics: An object:
  - ppm: The size of the map. Higher number = bigger players
  - bodies: A list of bodies. These are called platforms in the editor.
    - n: Platform's name
    - type: One of "s" (stationary), "d" (free-moving) or "k" (kinematic)
    - a, ad, av: Angle, angular drag and angular velocity
    - de, fric, ld, re: Density, friction, linear drag, bounciness
    - f_1, f_2, f_3, f_4, f_p: Whether or not this platform will collide with [A, B, C, D, players]
    - f_c: The platform's collide group. A=1, B=2, C=3, D=4
    - fr, fricp, bu: Whether or not [fixed rotation, fric players, anti-tunnel] is on
    - p, lv: Position and starting velocity, in the form of a two-key object with x and y
    - fx: Fixtures this platform is attached to
    - cf: Constant force:
      - x, y, ct: X force, Y force, and torque
     - w: Force direction - true=absolute, false=relative
  - fixtures: A list of fixture objects. Fixtures describe the properties of a shape.
    - n: Fixture's name.
    - d, ng, np, fp, ig: Whether or not [death, no grapple, no physics, fric players, inner grapple] is on
    - de, re, fr: Density, bounciness, and friction. Null means to inherit the value.
    - f: Fill color.
    - sh: Shape ID to attach to.
  - shapes: A list of shape objects.
    - stype: The shape's type: "bx"=rectangle, "ci"=circle, "po"=polygon
    - c: The shape's position, as an object of the form {"x":..., "y":...}
    - a: Angle
    - sk: Whether or not the shape shrinks over time. Not avaliable for polygons.
    - ### **For rectangles:**
    - w, h: Width and height
    - ### **For circles:**
    - r: Radius
    - ### **For polygons:**
    - s: Scale
    - v: Vertices, as a list of {"x":..., "y":...} objects
  - bro: A list of body IDs. The order here determines the display order.
  - joints: A list of joint objects.
    - ba: The body the join is on.
    - bb: The other body the joint connects with, or -1 for none.
    - *Other stuff probably, this object isn't completely documented.*

  
Code that decodes the map (may have changed in the Jul 22 update):
```js
/*
T.<something>() values:
getNewBoxShape: {"type":"bx","w":10,"h":40,"c":[0,0],"a":0,"sk":false}
getNewCircleShape: {"type":"ci","r":25,"c":[0,0],"sk":false}
getNewPolyShape: {"type":"po","v":[],"s":1,"a":0,"c":[0,0]}
getNewFixture: {"n":"Def Fix","fr":0.3,"fp":null,"re":0.8,"de":0.3,"f":5209260,"d":false,"np":false,"ng":false}
getNewBody: {"type":"s","n":"Unnamed","p":[0,0],"a":0,"fric":0.3,"fricp":false,"re":0.8,"de":0.3,"lv":[0,0],"av":0,"ld":0,"ad":0,"fr":false,"bu":false,"cf":{"x":0,"y":0,"w":true,"ct":0},"fx":[],"f_c":1,"f_p":true,"f_1":true,"f_2":true,"f_3":true,"f_4":true}
getNewSpawn: {"x":400,"y":300,"xv":0,"yv":0,"priority":5,"r":true,"f":true,"b":true,"gr":false,"ye":false,"n":"Spawn"}
getNewCapZone: {"n":"Cap Zone","ty":1,"l":10,"i":-1}
getNewRevoluteJoint: {"type":"rv","d":{"la":0,"ua":0,"mmt":0,"ms":0,"el":false,"em":false,"cc":false,"bf":0,"dl":true},"aa":[0,0]}
getNewDistanceJoint: {"type":"d","d":{"fh":0,"dr":0,"cc":false,"bf":0,"dl":true},"aa":[0,0],"ab":[0,0]}
getNewLPJJoint: {"type":"lpj","d":{"cc":false,"bf":0,"dl":true},"pax":0,"pay":0,"pa":0,"pf":0,"pl":0,"pu":0,"plen":0,"pms":0}
getNewLSJJoint: {"type":"lsj","d":{"cc":false,"bf":0,"dl":true},"sax":0,"say":0,"sf":0,"slen":0}
*/
T.decodeFromDatabase = function(map) { /* arg previously named R5H*/
            var a8k = [arguments]; // any variable like a8k[x] is just a normal variable, no need to worry about it being in a list
            b64mapdata = LZString.decompressFromEncodedURIComponent(map);
            G9b.w9b(); // afaik does nothing
            binaryReader = new v2k[35](); //a binary reader
            binaryReader.fromBase64(b64mapdata, false);//after this is the important bit
            map = T.getBlankMap(); /*blank map:
              {
                v: 1,
                s: {
                    re: false,
                    nc: false,
                    pq: 1,
                    gd: 25,
                    fl: false
                },
                physics: {
                    shapes: [],
                    fixtures: [],
                    bodies: [],
                    bro: [],
                    joints: [],
                    ppm: 12
                },
                spawns: [],
                capZones: [],
                m: {
                    a: 'noauthor',
                    n: 'noname',
                    dbv: 2,
                    dbid: -1,
                    authid: -1,
                    date: '',
                    rxid: 0,
                    rxn: '',
                    rxa: '',
                    rxdb: 1,
                    cr: [],
                    pub: false,
                    mo: ''
                }
              }
            */
            map.v = binaryReader.readShort();
            if (map.v > v2k[79].mapVersion) {
                throw new Error(G9b.z43(3465));
            }
            map.s.re = binaryReader.readBoolean();
            map.s.nc = binaryReader.readBoolean();
            if (map.v >= 3) {
                map.s.pq = binaryReader.readShort();
            }
            if (map.v >= 4 && map.v <= 12) {
                map.s.gd = binaryReader.readShort();
            } else if (map.v >= 13) {
                map.s.gd = binaryReader.readFloat();
            }
            if (map.v >= 9) {
                map.s.fl = binaryReader.readBoolean();
            }
            map.m.rxn = binaryReader.readUTF();
            map.m.rxa = binaryReader.readUTF();
            map.m.rxid = binaryReader.readUint();
            map.m.rxdb = binaryReader.readShort();
            map.m.n = binaryReader.readUTF();
            map.m.a = binaryReader.readUTF();
            if (map.v >= 10) {
                map.m.vu = binaryReader.readUint();
                map.m.vd = binaryReader.readUint();
            }
            if (map.v >= 4) {
                cr_count = binaryReader.readShort();
                for (cr_iterator = 0; cr_iterator < cr_count; cr_iterator++) {
                    map.m.cr.push(binaryReader.readUTF());
                }
            } 
            if (map.v >= 5) {
                map.m.mo = binaryReader.readUTF();
                map.m.dbid = binaryReader.readInt();
            }
            if (map.v >= 7) {
                map.m.pub = binaryReader.readBoolean();
            }
            if (map.v >= 8) {
                map.m.dbv = binaryReader.readInt();
            }
            map.physics.ppm = binaryReader.readShort();
            bro_count = binaryReader.readShort();
            for (bro_iterator = 0; bro_iterator < bro_count; bro_iterator++) {
                map.physics.bro[bro_iterator] = binaryReader.readShort();
            }
            shape_count = binaryReader.readShort();
            for (shape_iterator = 0; shape_iterator < shape_count; shape_iterator++) {
                shape_type = binaryReader.readShort();
                if (shape_type == 1) {
                    map.physics.shapes[shape_iterator] = T.getNewBoxShape();
                    map.physics.shapes[shape_iterator].w = binaryReader.readDouble();
                    map.physics.shapes[shape_iterator].h = binaryReader.readDouble();
                    map.physics.shapes[shape_iterator].c = [binaryReader.readDouble(), binaryReader.readDouble()];
                    map.physics.shapes[shape_iterator].a = binaryReader.readDouble();
                    map.physics.shapes[shape_iterator].sk = binaryReader.readBoolean();
                }
                if (shape_type == 2) {
                    map.physics.shapes[shape_iterator] = T.getNewCircleShape();
                    map.physics.shapes[shape_iterator].r = binaryReader.readDouble();
                    map.physics.shapes[shape_iterator].c = [binaryReader.readDouble(), binaryReader.readDouble()];
                    map.physics.shapes[shape_iterator].sk = binaryReader.readBoolean();
                }
                if (shape_type == 3) {
                    map.physics.shapes[shape_iterator] = T.getNewPolyShape();
                    map.physics.shapes[shape_iterator].s = binaryReader.readDouble();
                    map.physics.shapes[shape_iterator].a = binaryReader.readDouble();
                    map.physics.shapes[shape_iterator].c = [binaryReader.readDouble(), binaryReader.readDouble()];
                    point_count = binaryReader.readShort();
                    map.physics.shapes[shape_iterator].v = [];
                    for (point_iterator = 0; point_iterator < point_count; point_iterator++) {
                        map.physics.shapes[shape_iterator].v.push([binaryReader.readDouble(), binaryReader.readDouble()]);
                    }
                }
            }
            a8k[31] = binaryReader.readShort();
            for (a8k[89] = 0; a8k[89] < a8k[31]; a8k[89]++) {
                map.physics.fixtures[a8k[89]] = T.getNewFixture();
                map.physics.fixtures[a8k[89]].sh = binaryReader.readShort();
                map.physics.fixtures[a8k[89]].n = binaryReader.readUTF();
                map.physics.fixtures[a8k[89]].fr = binaryReader.readDouble();
                if (map.physics.fixtures[a8k[89]].fr == Number.MAX_VALUE) {
                    map.physics.fixtures[a8k[89]].fr = null;
                }
                a8k[22] = binaryReader.readShort();
                if (a8k[22] == 0) {
                    map.physics.fixtures[a8k[89]].fp = null;
                }
                if (a8k[22] == 1) {
                    map.physics.fixtures[a8k[89]].fp = false;
                }
                if (a8k[22] == 2) {
                    map.physics.fixtures[a8k[89]].fp = true;
                }
                map.physics.fixtures[a8k[89]].re = binaryReader.readDouble();
                if (map.physics.fixtures[a8k[89]].re == Number.MAX_VALUE) {
                    map.physics.fixtures[a8k[89]].re = null;
                }
                map.physics.fixtures[a8k[89]].de = binaryReader.readDouble();
                if (map.physics.fixtures[a8k[89]].de == Number.MAX_VALUE) {
                    map.physics.fixtures[a8k[89]].de = null;
                }
                map.physics.fixtures[a8k[89]].f = binaryReader.readUint();
                map.physics.fixtures[a8k[89]].d = binaryReader.readBoolean();
                map.physics.fixtures[a8k[89]].np = binaryReader.readBoolean();
                if (map.v >= 11) {
                    map.physics.fixtures[a8k[89]].ng = binaryReader.readBoolean();
                }
                if (map.v >= 12) {
                    map.physics.fixtures[a8k[89]].ig = binaryReader.readBoolean();
                }
            }
            a8k[41] = binaryReader.readShort();
            for (a8k[20] = 0; a8k[20] < a8k[41]; a8k[20]++) {
                map.physics.bodies[a8k[20]] = T.getNewBody(); // this returns:
                // {'type': 's', 'n': 'Unnamed', 'p': [0, 0], 'a': 0, 'fric': 0.3, 'fricp': False, 're': 0.8, 'de': 0.3, 'lv': [0, 0], 'av': 0, 'ld': 0, 'ad': 0, 'fr': False, 'bu': False, 'cf': {'x': 0, 'y': 0, 'w': True, 'ct': 0}, 'fx': [], 'f_c': 1, 'f_p': True, 'f_1': True, 'f_2': True, 'f_3': True, 'f_4': True}
                map.physics.bodies[a8k[20]].type = binaryReader.readUTF();
                map.physics.bodies[a8k[20]].n = binaryReader.readUTF();
                map.physics.bodies[a8k[20]].p = [binaryReader.readDouble(), binaryReader.readDouble()];
                map.physics.bodies[a8k[20]].a = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].fric = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].fricp = binaryReader.readBoolean();
                map.physics.bodies[a8k[20]].re = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].de = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].lv = [binaryReader.readDouble(), binaryReader.readDouble()];
                map.physics.bodies[a8k[20]].av = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].ld = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].ad = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].fr = binaryReader.readBoolean();
                map.physics.bodies[a8k[20]].bu = binaryReader.readBoolean();
                map.physics.bodies[a8k[20]].cf.x = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].cf.y = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].cf.ct = binaryReader.readDouble();
                map.physics.bodies[a8k[20]].cf.w = binaryReader.readBoolean();
                map.physics.bodies[a8k[20]].f_c = binaryReader.readShort();
                map.physics.bodies[a8k[20]].f_1 = binaryReader.readBoolean();
                map.physics.bodies[a8k[20]].f_2 = binaryReader.readBoolean();
                map.physics.bodies[a8k[20]].f_3 = binaryReader.readBoolean();
                map.physics.bodies[a8k[20]].f_4 = binaryReader.readBoolean();
                if (map.v >= 2) {
                    map.physics.bodies[a8k[20]].f_p = binaryReader.readBoolean();
                }
                a8k[50] = binaryReader.readShort();
                for (a8k[66] = 0; a8k[66] < a8k[50]; a8k[66]++) {
                    map.physics.bodies[a8k[20]].fx.push(binaryReader.readShort());
                }
            }
            a8k[48] = binaryReader.readShort();
            for (a8k[36] = 0; a8k[36] < a8k[48]; a8k[36]++) {
                map.spawns[a8k[36]] = T.getNewSpawn();
                a8k[80] = map.spawns[a8k[36]];
                a8k[80].x = binaryReader.readDouble();
                a8k[80].y = binaryReader.readDouble();
                a8k[80].xv = binaryReader.readDouble();
                a8k[80].yv = binaryReader.readDouble();
                a8k[80].priority = binaryReader.readShort();
                a8k[80].r = binaryReader.readBoolean();
                a8k[80].f = binaryReader.readBoolean();
                a8k[80].b = binaryReader.readBoolean();
                a8k[80].gr = binaryReader.readBoolean();
                a8k[80].ye = binaryReader.readBoolean();
                a8k[80].n = binaryReader.readUTF();
            }
            a8k[40] = binaryReader.readShort();
            for (a8k[18] = 0; a8k[18] < a8k[40]; a8k[18]++) {
                map.capZones[a8k[18]] = T.getNewCapZone();
                map.capZones[a8k[18]].n = binaryReader.readUTF();
                map.capZones[a8k[18]].l = binaryReader.readDouble();
                map.capZones[a8k[18]].i = binaryReader.readShort();
                if (map.v >= 6) {
                    map.capZones[a8k[18]].ty = binaryReader.readShort();
                }
            }
            a8k[39] = binaryReader.readShort();
            for (a8k[94] = 0; a8k[94] < a8k[39]; a8k[94]++) {
                a8k[75] = binaryReader.readShort();
                if (a8k[75] == 1) {
                    map.physics.joints[a8k[94]] = T.getNewRevoluteJoint();
                    a8k[53] = map.physics.joints[a8k[94]];
                    a8k[53].d.la = binaryReader.readDouble();
                    a8k[53].d.ua = binaryReader.readDouble();
                    a8k[53].d.mmt = binaryReader.readDouble();
                    a8k[53].d.ms = binaryReader.readDouble();
                    a8k[53].d.el = binaryReader.readBoolean();
                    a8k[53].d.em = binaryReader.readBoolean();
                    a8k[53].aa = [binaryReader.readDouble(), binaryReader.readDouble()];
                }
                if (a8k[75] == 2) {
                    map.physics.joints[a8k[94]] = T.getNewDistanceJoint();
                    a8k[27] = map.physics.joints[a8k[94]];
                    a8k[27].d.fh = binaryReader.readDouble();
                    a8k[27].d.dr = binaryReader.readDouble();
                    a8k[27].aa = [binaryReader.readDouble(), binaryReader.readDouble()];
                    a8k[27].ab = [binaryReader.readDouble(), binaryReader.readDouble()];
                }
                if (a8k[75] == 3) {
                    map.physics.joints[a8k[94]] = T.getNewLPJJoint();
                    a8k[23] = map.physics.joints[a8k[94]];
                    a8k[23].pax = binaryReader.readDouble();
                    a8k[23].pay = binaryReader.readDouble();
                    a8k[23].pa = binaryReader.readDouble();
                    a8k[23].pf = binaryReader.readDouble();
                    a8k[23].pl = binaryReader.readDouble();
                    a8k[23].pu = binaryReader.readDouble();
                    a8k[23].plen = binaryReader.readDouble();
                    a8k[23].pms = binaryReader.readDouble();
                }
                if (a8k[75] == 4) {
                    map.physics.joints[a8k[94]] = T.getNewLSJJoint();
                    a8k[47] = map.physics.joints[a8k[94]];
                    a8k[47].sax = binaryReader.readDouble();
                    a8k[47].say = binaryReader.readDouble();
                    a8k[47].sf = binaryReader.readDouble();
                    a8k[47].slen = binaryReader.readDouble();
                }
                map.physics.joints[a8k[94]].ba = binaryReader.readShort();
                map.physics.joints[a8k[94]].bb = binaryReader.readShort();
                map.physics.joints[a8k[94]].d.cc = binaryReader.readBoolean();
                map.physics.joints[a8k[94]].d.bf = binaryReader.readDouble();
                map.physics.joints[a8k[94]].d.dl = binaryReader.readBoolean();
                ;
            }
            return map;
        }
```
