<template>
  <div id="body" class="generalContainer">
  <p v-if="showGenerate" id="explanation">It's not difficult to understand that people disagree. It is a reality of the human condition.<br><br>
  However, for many of us, it can be astonishingly difficult to understand when people disagree with established scientific facts.<br><br>
  We created this project in efforts to better understand people who disagree with science.<br><br>
  Using AI, the system draws on real peoples' words about science, rendering them into artistic visualizations, in turn, offering glimpses at their imaginings of science--to "see" through their eyes.<br><br>
  Click <button id="generateButton" @click="doImageThings">Imagine Science</button> and see for yourself.
  </p>
  <span v-if="showImages" id="toggle"><button id="forButton" @click="toggle" class="toggle">For {{typeOfScience}}</button><button id="againstButton" @click="toggle2" class="toggle">Against {{typeOfScience}}</button></span>
	<p v-if="showImages" id="imagesContainer">
		<span class="textPrompt" id="pro"><section class="typeOfScience">{{websiteName}}</section><br><section class="websiteName">Source: {{source1}}</section><br><span id="stanceExplanation1">{{stanceExplanation}} </span><br><br> <span class="originaTextandPrompt"><b>Sampled Text</b><br> {{originalText1}}<br><br><b>Image Description (Generated from Sampled Text)</b><br>{{textPrompt1}}</span></span><section class="images" id="image1"><img class="loadingGif" v-if="loadingGif" src="/loading.gif"></section>\n
		<span class="textPrompt" id="anti"><section class="typeOfScience">{{websiteName2}}</section><br><section class="websiteName">Source: {{source2}}</section><br><span id="stanceExplanation2">{{stanceExplanation2}} </span><br><br> <span class="originaTextandPrompt"><b>Sampled Text</b><br> {{originalText2}}<br><br><b>Image Description (Generated from Sampled Text)</b><br>{{textPrompt2}}</span></span><section class="images" id="image2"><img class="loadingGif" v-if="loadingGif" src="/loading.gif"></section>
	</p>
  </div>
</template>

<script>
import axios from "axios";
import dotenv from "dotenv";
dotenv.config();
//import OpenAI from "openai";
export default {
  name: "imagineScience",
  props: {},
  data() {
    return {
      msg: "Imagining Science",
      showGenerate: true,
      showImages: false,
      apiKEY: process.env.VUE_APP_SUPER_SECRET,
      imageURL: "",
      imageURL2: "",
      typeOfScience: "Vaccination",
      websiteName: "Vaccinate Your Family", 
      websiteName2: "The National Vaccine Information Center", 
      loadingGif: false,
      stanceExplanation: "Vaccinate Your Children is an organization that believes that vaccines are necessary and safe.",
      stanceExplanation2: "National Vaccine Information Center is an organization suspicious of the safety of vaccines.",
      source1: "Vaccinate Your Family Mission Statement (https://vaccinateyourfamily.org/our-mission-values/)",
      source2: "National Vaccine Information Center Mission Statement (https://www.nvic.org/about/mission-vision)",
      textPrompt1: "In the garden of protection, 'Vaccinate Your Family' blooms with a purpose. Each day, they nurture the fragile buds of awareness and understanding, tending to the safety and health of all ages. Their advocacy weaves a vibrant tapestry of life, where policies ensure timely vaccination for all.", 
      textPrompt2: "NVIC, a guardian of knowledge, weaves a vibrant tapestry of protection. In their symphony of advocacy, the delicate notes of informed consent and the vibrant hues of freedom dance together. Flexible exemptions interlace, painting a vision of individual rights and collective well-being.", 
      originalText1: '"Vaccinate Your Family’s mission is to protect people of all ages from vaccine-preventable diseases. Each day, we work to achieve our mission by: Raising awareness of the critical need for timely immunizationsIncreasing the public’s understanding of the benefits of vaccinesIncreasing confidence in the safety of vaccinesEnsuring that all families have access to lifesaving vaccinesAdvocating for policies that support timely vaccination."', 
      originalText2: '"The National Vaccine Information Center (NVIC) is dedicated to preventing vaccine injuries and deaths through public education and advocating for informed consent protections in medical policies and public health laws. NVIC defends the human right to freedom of thought and conscience and supports the inclusion of flexible medical, religious and conscientious belief exemptions in vaccine policies and laws."'
    };
  },

  created: function () {},

  methods: {
	doImageThings: function () {
		this.showGenerate = false
		this.showImages = true
		this.loadingGif = true
		this.getImageBasedOnText()
		this.getImageBasedOnText2()
	},

    getImageBasedOnText: function () {
      const client = axios.create({
            headers: {
              Authorization: "Bearer " + this.apiKEY,
            },
          });

          const params = {
            model: "dall-e-2",
            prompt:
              'Make an image that represents the content of the following statement. Statement: ' +
              this.textPrompt2,
              n: 1,
              size: "512x512",
          };

          client
            .post("https://api.openai.com/v1/images/generations", params)
            .then((result) => {
              this.imageURL = result.data.data[0].url


              var div = document.getElementById("image1");
              var p = document.createElement("img");
              p.src = this.imageURL;
              div.append(p);
            })
            .catch((error) => {
              console.log("image 1" + error);
              this.msg = "image 2" + error;
            });
    },
    
    getImageBasedOnText2: function () {
      const client = axios.create({
            headers: {
              Authorization: "Bearer " + this.apiKEY,
            },
          });

          const params = {
            model: "dall-e-2",
            prompt:
              'Make an image that represents the content of the following statement. Statement: ' +
              this.textPrompt2,
              n: 1,
              size: "512x512",
          };

          client
            .post("https://api.openai.com/v1/images/generations", params)
            .then((result) => {
              this.msg = "Imaginings of " + this.typeOfScience;
              this.loadingGif = false
              this.imageURL2 = result.data.data[0].url


              var div2 = document.getElementById("image2");
              var p2 = document.createElement("img");
              p2.src = this.imageURL2;
              div2.append(p2);
              document.getElementById("forButton").focus();
            })
            .catch((error) => {
              console.log("image 2" + error);
              this.msg ="image 2" +  error;
            });
    },
    
    toggle: function () {
    document.getElementById("pro").style.display = "block";
    document.getElementById("anti").style.display = "none";
    document.getElementById("image1").style.display = "block";
    document.getElementById("image2").style.display = "none";
    }, 
    
    toggle2: function () {
    document.getElementById("anti").style.display = "block";
    document.getElementById("pro").style.display = "none";
    document.getElementById("image2").style.display = "block";
    document.getElementById("image1").style.display = "none";
    }

  }, //
}; //
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#title {
font-size: 50px;
color: #FFC300;
background: #7014f2; 
}
#imagesContainer {
display: flex; 
width: 100%;
}
.textPrompt {
color: white;
background: rgba(0, 0, 0, 0.6);
padding: 20px; 
font-size: 20px; 
text-align: left; 
border: none;
}
.images {
color: white;
background: rgba(0, 0, 0, 0.6);
padding: 20px; 
font-size: 20px; 
text-align: center; 
border: none;
width: 100%;
padding-bottom: 3.75%;
}
.loadingGif {
}
.typeOfScience {
color: black;
font-size: 35px;
background-color: #7014f2;
font-weight: bold;
text-align: center;
border: none; 

}
.websiteName {
font-size: 25px;
color: black; 
font-weight: bold;
background: #ccff00;
text-align: center;
border: none;
}
.originaTextandPrompt {
}
.toggle {
background: rgba(0, 0, 0, 0.3);
border: none;
height: 100px;
width: 40%;
font-size: 50px;
font-weight: bold;
}
#forButton:focus { 
    background-color:#66FF00;    
}
#againstButton:focus {  
    background-color:#FF007F;    
}
#stanceExplanation1 {
color: #66FF00;
font-weight: bold;
font-size: 25px;
}
#stanceExplanation2 {
color: #FF007F;
font-weight: bold;
}
#toggle {
width: 100%;
}
#generateButton {
background: #ccff00;
border: none;
height: 50px;
font-size: 30px;
font-weight: bold;
}
#explanation {
color: white; 
font-size: 30px;
text-align: left; 
width: 85%;
margin-top: 10%;
margin-left: 7%;
font-weight: bold;
}
#anti, #image2 {
display: none;
}

</style>
