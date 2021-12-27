<template>
<div :style="{ backgroundImage: `url(${require('@/assets/' + image)})`, backgroundRepeat: 'no-repeat',
backgroundSize: 'cover',marginTop: '-60px'}">

<form id="box" v-if="show">
    <h1 id="header">{{munka}} pozícióra jelentkezés</h1>
    <hr class="vonal">
    <div>
        <label><h3>Neve:&nbsp;</h3></label>
        <input class="inputfield" type="text" v-model.trim="name" @change="teszt1(); jelentkez()" placeholder="Kiss János" minlength="2" maxlength="50" required 
        pattern="[A-z\u00C0-\u00FF]{2,}\.?\s[A-z\u00C0-\u00FF]{2,}(\s[A-z\u00C0-\u00FF]{2,})*$" title="Kérjük valódi nevet adjon meg! Pl.: Szekeres Attila">
        <br>
        <span class="error" v-if="helyesName">A megadott név helytelen!</span>
    </div>
    <div>
        <label><h3>E-mail címe:&nbsp;</h3></label>
        <input class="inputfield" type="text" v-model.trim="email" @change="teszt2(); jelentkez()" placeholder="cim@freemail.hu"  required pattern="[A-z\u00C0-\u00FF0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$" title="Az e-mail cím formátuma nem megfelelő! Pl.: cim@freemail.hu">
        <br>
        <span class="error" v-if="helyesEmail">A megadott e-mail helytelen!</span>
    </div>
    <div>
        <label><h3>Telefonszáma:&nbsp;</h3></label>
        <input class="inputfield" type="number" v-model.trim="telefon"  @change="teszt3(); jelentkez()" placeholder="+36 30 995 6648" required  pattern="\+?[03]6[237]0\d{7}$" title="Mobiltelefon számát adja meg! Pl.: +36 30 995 6648">
        <br>
        <span class="error" v-if="helyesTelefon">A megadott telefonszám helytelen!</span>
    </div>
    <br>
    <hr class="vonal">
    <div id="lakcim">
        <label><h2>Lakcíme:&nbsp;</h2></label>
        <br>
        <label class="col-6 col-sm-6 col-md-6"><h5>Irányítószáma:</h5></label>
        <input class="inputfield col-6 col-sm-6 col-md-6" type="number" v-model.trim="irszam"  @change="teszt4(); jelentkez()"  placeholder="6721" required pattern="[1-9]{1}[0-9]{3}" title="Kérjük valódi irányítószámot adjon meg! Pl.: 6721">
        <br>
        <span class="error col-6 col-sm-6 col-md-6" v-if="helyesIrszam">A megadott Irányítószám helytelen!</span>

        <br>
        <label class="col-6 col-sm-6 col-md-6"><h5>Település:</h5></label>
        <input class="inputfield col-6 col-sm-6 col-md-6" type="text" v-model.trim="telepules" minlength="2"  @change="teszt5(); jelentkez()" required placeholder="Budapest"   pattern="[A-z\u00C0-\u00FF]{3,}(\s[A-z\u00C0-\u00FF]{2,})*$" title="Kérjük magyarországi települést adjon meg! Pl.: Budapest">
        <br>
        <span class="error col-6 col-sm-6 col-md-6" v-if="helyesTelepules">A megadott település helytelen!</span>

        <br>
        <label class="col-6 col-sm-6 col-md-6"><h5>Utca:</h5></label>
        <input class="inputfield col-6 col-sm-6 col-md-6" type="text" v-model.trim="utca" minlength="2" @change="teszt6(); jelentkez()"  placeholder="József Attila" required pattern="[A-z\u00C0-\u00FF]{3,}(\s[A-z\u00C0-\u00FF]{2,}\.?)*$">
        <br>
        <span class="col-6 col-sm-6 col-md-6 error" v-if="helyesUtca">A megadott utca helytelen!</span>

        <br>
        <label class="col-6 col-sm-6 col-md-6"><h5>Házszám:</h5></label>
        <input class="inputfield col-6 col-sm-6 col-md-6" type="text" v-model.trim="hazszam"  @change="teszt7(); jelentkez()" placeholder="29" required  pattern="[A-z\u00C0-\u00FF]?\/?[0-9]{1,3}\/?[A-z\u00C0-\u00FF]?">
        <br>
        <span class="error col-6 col-sm-6 col-md-6" v-if="helyesHazszam">A megadott házszám helytelen!</span>

    </div>
    <hr class="vonal">
    <br>
    <div id="pozicio">
      <label><h3>Pozíció amire jelentkezik:&nbsp;</h3></label>
      <select required v-model="pozicio" @change="jelentkez(); csere()">
        <option value="">Kötelező választani!</option>
        <option value="frontend">Frontend</option>
        <option value="backend">Backend</option>
        <option value="fullstack">Full stack</option>
      </select>
    </div>
    <div>
        <label id="tapasztalat"><h3>Tapasztalata:&nbsp;&nbsp;</h3></label>
        <br>
        <textarea rows="8" cols="50" type="text" maxlength="500" placeholder="Korábbi tapasztalatai..." @keyup="szamlalo()" v-model="tapasztalat"></textarea>
        <br>
        <span id="szamlalo">{{hossz}}</span>
    </div>

    <hr class="vonal">

    <div id="ismtech">
        <label><h3>Ismert technológiák:&nbsp;&nbsp;</h3></label>
        <input class="inputfield" type="text" v-model="ismertTech" @keyup="tombhozAd">
        <br>
        <div v-for="elem in ismertTechnologiak" :key="elem" id="ismertTechnologiak">
           <span @click="torles(elem)">{{elem}}</span></div>
    </div>

    <input id="gomb" type="button" value="Jelentkezés" v-if="jelentkezes" @click="lead">
</form>
</div>

<div v-if="!show" :style="{ backgroundImage: `url(${require('@/assets/' + image)})`, backgroundRepeat: 'no-repeat',
backgroundSize: 'cover',marginTop: '-60px', height:'1200px'}" >

 <form id="leadas">
    <h1>Helyesek az adatok?</h1>
    <table>
        <tr>
            <th>Megnevezés</th>
            <th>Adatai</th>
        </tr>
        <tr>
            <td>Neve:</td>
            <td><p>{{name}}</p></td>
        </tr>
        <tr>
            <td>E-mail címe:</td>
            <td><p>{{email}}</p></td>
        </tr>
        <tr>
            <td>Telefonszáma:</td>
            <td><p>{{telefon}}</p></td>
        </tr>
        <tr>
            <td>Címe: </td>
            <td><p>{{irszam}} {{telepules}} {{utca}} {{hazszam}}</p></td>
        </tr>
        <tr>
            <td>Kiválasztott pozíció:</td>
            <td><p>{{pozicio}}</p></td>
        </tr>
        <tr>
            <td>Tapasztalatai:</td>
            <td>
                <p>{{tapasztalat}}</p>
            </td>
        </tr>
        <tr>
            <td>Ismert technológiák:</td>
            <td>
                <p v-for="ismertTech in ismertTechnologiak" :key="ismertTech">{{ismertTech}}</p>
            </td>
        </tr>
    </table>
    <input id="gomb2" type="submit" value="Igen">
</form>
</div>
</template>

<script>

export default {
    data(){
        return{
            regName: /[A-z\u00C0-\u00FF]{2,}\.?\s[A-z\u00C0-\u00FF]{2,}(\s[A-z\u00C0-\u00FF]{2,})*$/,
            helyesName: false,
            regEmail: /[A-z\u00C0-\u00FF0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$/,
            helyesEmail: false,
            regTelefon: /\+?[03]6[237]0\d{7}$/,
            helyesTelefon: false,
            regIrszam: /^[1-9]{1}\d{3}$/,
            helyesIrszam: false,
            regTelepules: /[A-z\u00C0-\u00FF]{3,}(\s[A-z\u00C0-\u00FF]{2,})*$/,
            helyesTelepules: false,
            regUtca: /[A-z\u00C0-\u00FF]{3,}(\s[A-z\u00C0-\u00FF]{2,}\.?)*$/,
            helyesUtca:false,
            regHazszam: /^[A-z\u00C0-\u00FF]?\/?\d{1,3}$\/?[A-z\u00C0-\u00FF]?$/,
            helyesHazszam: false,
            show: true,
            jelentkezes: false,
            name: "",
            email: "",
            telefon: "",
            irszam: "",
            telepules: "",
            utca: "",
            hazszam: "",
            hossz: 500,
            tapasztalat: "",
            munka: "Fejlesztői",
            image: "alap.jpg",
            pozicio: "",
            ismertTech:"",
            ismertTechnologiak: []
        }
    },
    methods:{
        csere(){
            switch(this.pozicio) {
                case "frontend":
                    this.munka="Frontend",
                    this.image="frontend.jpg"
                    break;
                case "backend":
                    this.munka="Backend",
                    this.image="backend.png"
                    break;
                case "fullstack":
                    this.munka="Full stack",
                    this.image="fullstack.jpg"
                    break;
                default: 
            }
        },
        teszt1(){
            if (this.regName.test(this.name)) {
                this.helyesName=false
            } else {this.helyesName=true}},
        teszt2(){
            if (this.regEmail.test(this.email)) {
                this.helyesEmail=false
            } else {this.helyesEmail=true}},
        teszt3(){
            if (this.regTelefon.test(this.telefon)) {
                this.helyesTelefon=false
            } else {this.helyesTelefon=true}},
        teszt4(){
            if (this.regIrszam.test(this.irszam)) {
                this.helyesIrszam=false
            } else {this.helyesIrszam=true}},
        teszt5(){
            if (this.regTelepules.test(this.telepules)) {
                this.helyesTelepules=false
            } else {this.helyesTelepules=true}},
        teszt6(){
            if (this.regUtca.test(this.utca)) {
                this.helyesUtca=false
            } else {this.helyesUtca=true}},
        teszt7(){
            if (this.regHazszam.test(this.hazszam)) {
                this.helyesHazszam=false
            } else {this.helyesHazszam=true}
        },
        szamlalo(){
            this.hossz=500
            this.hossz=this.hossz-this.tapasztalat.length
        },
        tombhozAd(x) {
            if (x.key === " " && this.ismertTech) {
                if (!this.ismertTechnologiak.includes(this.ismertTech)){
                    this.ismertTechnologiak.push(this.ismertTech)
                    this.ismertTech=""
                }
                else this.ismertTech=""
            }
        },
        torles(elem) {
            this.ismertTechnologiak = this.ismertTechnologiak.filter((x)=> {
                return elem !== x
            })
        },
        jelentkez(){
                if (this.name != "" && this.email != "" && this.telefon != "" &&
                this.irszam != "" && this.telepules != "" && this.utca != "" &&
                this.hazszam != "" && this.pozicio != "" && !this.helyesName && !this.helyesEmail && !this.helyesTelefon
                && !this.helyesIrszam && !this.helyesTelepules && !this.helyesUtca && !this.helyesHazszam && this.pozicio!="") {
                    this.jelentkezes=true
                } else {this.jelentkezes=false}
            },
        lead(){
            this.show=false
        }
    }
}
</script>

<style scoped>
#box{
    background: rgb(230, 227, 227);
    opacity: 80%;
    margin: auto;
    max-width: 800px;
    border: rgba(255, 255, 255, 0.678) solid 3px;
    border-radius: 10px;
    box-shadow: 10px 10px 5px 0px rgba(0,0,0,0.75);
    color: black;
}
#header{
    margin: 10px 30px 10px 30px;
    padding: 50px;
    background: rgba(248, 197, 101, 0.74);
    border-top: black 1px solid;
    border-bottom: black 1px solid;
}
#lakcim{
    padding-bottom: 20px;
}
.vonal{
    height: 5px;
    background: rgb(255, 174, 0);;
}
#pozicio, #ismtech{
    margin-bottom: 50px;
}
#gomb{
    padding: 10px 30px 10px 30px;
    background: rgba(248, 197, 101, 0.74);;
    border-radius: 10px;
    box-shadow: 10px 10px 5px 0px rgba(0,0,0,0.75);
    margin-bottom: 50px;
    font-size: 2rem;
    margin-top: 50px;
}
input,textarea,select{
    background:rgb(230, 227, 227);
    border-radius: 5px;
}
.inputfield{
    max-width: 200px;
    border-right: lightgray;
    border-left: lightgray;
    border-top: lightgray;
}
select{
    height: 50px;
    font-size: 30px;
}
textarea,select{
    border-right: lightgray;
    border-left: lightgray;
}
input:hover,textarea:hover,select:hover{
    background-color: rgba(248, 197, 101, 0.74);
}
h2{
    margin-bottom: 50px;
}
#gomb:hover{
    background: orange;
    box-shadow: 15px 15px 10px 0px rgba(0,0,0,0.75);
}
#tapasztalat{
    margin-left: -250px;
}
#ismertTechnologiak{
    cursor: pointer;
    color:black;
    font-weight: bold;
    display: inline-block;
    border:black solid 1px;
    margin: 20px 10px 0 0;
    padding: 6px 12px;
    background: rgba(248, 197, 101, 0.74);
    border-radius: 20px;
}
#szamlalo{
    margin-left: 380px;
}
td{
    font-weight: bold;
    color: brown;
    max-width: 400px;
    overflow: hidden;
    text-overflow: ellipsis;
}
td > p{
    margin-top: 15px;
    color: black;
}
th, td{
    background-color: #a0f3d094;
}
table,th,td{
    table-layout: auto;
    margin: auto;
    border: 1px solid grey;
    border-collapse: collapse;
}
#leadas{
    opacity: 90%;
    margin: auto;
    border: black solid 1px;
    border-radius: 5px;
    background: #eee;
    width: 600px;
}
#gomb2{
    background: rgba(248, 197, 101, 0.74);
    margin-top: 15px;
    margin-bottom: 20px;
    font-weight: bold;
    border-radius: 5px;
    box-shadow: 5px 5px 5px 0px rgba(0,0,0,0.75);
}
.error{
    color: red;
    font-weight: bold;
    font-size: 1.2rem;
}

</style>