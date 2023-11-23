<template>
  <div id="body" class="generalContainer">
  <p v-if="showGenerate" id="explanation">
  Using AI, this project draws on real peoples' words about science to render them into artistic visualizations. <br><br>Our hope is that the imagery will offer glimpses at science through others' eyes, in turn, supporting better understanding of those moments in which people disagree about science.<br><br>
  Click <button id="generateButton" @click="doImageThings">Imagine Science</button> and see for yourself.<br><br><br><br><br><br><br><br><br>
  </p>
    
  <span v-if="showImages" id="toggle"><button id="forButton" class="toggle" @click="trigger" >For<br>{{typeOfScience}}</button><button id="againstButton" class="toggle" @click="trigger2">Against<br>{{typeOfScience}}</button></span><br><br>
  <span v-if="showImages" id="buttonArray"><button id="vaccinationButton" @click="vaccination">Vaccination</button><button id="climateChangeButton" @click="climateChange">Climate Change</button><button id="roundEarthButton" @click="roundEarth">Round Earth</button></span><br><br>
  <button v-if="showImages" id="generateButton2" @click="doImageThings2">See New Images</button>

	<p v-if="showImages" id="imagesContainer">
		<span class="textPrompt" id="pro"><section id="websiteName1" class="typeOfScience">{{websiteName}}</section><br><span id="stanceExplanation1">{{stanceExplanation}} </span><br><br><section class="images" id="image1"><img class="loadingGif" v-if="loadingGif" :src="imgURL1" ></section><br> <span class="originaTextandPrompt"><b>Sampled Text</b><br> {{originalText1}}<br><br><b>Image Description (Generated from Sampled Text)</b><br>{{textPrompt1}}</span><br><br><section class="websiteName">Source: {{source1}}</section><br></span>
		<span class="textPrompt" id="anti"><section id="websiteName2" class="typeOfScience">{{websiteName2}}</section><br><span id="stanceExplanation2">{{stanceExplanation2}} </span><br><br><section class="images" id="image2"><img class="loadingGif" v-if="loadingGif" :src="imgURL2" ></section><br> <span class="originaTextandPrompt"><b>Sampled Text</b><br> {{originalText2}}<br><br><b>Image Description (Generated from Sampled Text)</b><br>{{textPrompt2}}</span><br><br><section class="websiteName">Source: {{source2}}</section><br></span>
	</p>
  </div>
</template>

<script>
//import axios from "axios";
//import dotenv from "dotenv";
//dotenv.config();
//import OpenAI from "openai";
export default {
  name: "imagineScience",
  props: {},
  data() {
    return {
      msg: "Imagining Science",
      showGenerate: true,
      showImages: false,
      apiKEY: "placeholder",//process.env.VUE_APP_SUPER_SECRET,
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
      originalText2: '"The National Vaccine Information Center (NVIC) is dedicated to preventing vaccine injuries and deaths through public education and advocating for informed consent protections in medical policies and public health laws. NVIC defends the human right to freedom of thought and conscience and supports the inclusion of flexible medical, religious and conscientious belief exemptions in vaccine policies and laws."', 
      scienceType: "vaccination",
      itemNumber: 0,
      imgURL1: "", 
      imgURL2: "", 
      prompt: "", 
      promptAndImageData: 				[{
   "vaccination": {
      "for_website_name": "Vaccinate Your Family (.org)",
      "against_website_name": "National Vaccine Information Center (.org)",
      "for_original_text": "Vaccinate Your Family's mission is to protect people of all ages from vaccine-preventable diseases. Each day, we work to achieve our mission by: Raising awareness of the critical need for timely immunizationsIncreasing the public's understanding of the benefits of vaccinesIncreasing confidence in the safety of vaccinesEnsuring that all families have access to lifesaving vaccinesAdvocating for policies that support timely vaccination.",
      "against_original_text": "The National Vaccine Information Center (NVIC) is dedicated to preventing vaccine injuries and deaths through public education and advocating for informed consent protections in medical policies and public health laws. NVIC defends the human right to freedom of thought and conscience and supports the inclusion of flexible medical, religious and conscientious belief exemptions in vaccine policies and laws.",
      "for_prompt1": "In the garden of health and compassion, 'Vaccinate Your Family' tends to the blossoms of protection. Their daily care nurtures awareness and understanding, like sunlight on petals, while advocating for policies that ensure the whole world has access to life-saving vaccines, a vibrant tapestry of safeguarded well-being.",
      "against_prompt1": "NVIC, a guardian of knowledge, weaves a vibrant tapestry of protection. In their symphony of advocacy, the delicate notes of informed consent and the vibrant hues of freedom dance together. Flexible exemptions interlace, painting a vision of individual rights and collective well-being.",
      "for_prompt2": "In the tapestry of health and safety, 'Vaccinate Your Family' emerges as a vigilant guardian. Their daily quest paints a vivid image of raising awareness, fostering understanding, and planting seeds of confidence. Policies, like guiding stars, ensure all families access the lifesaving treasure of vaccines.",
      "against_prompt2": "NVIC, a guardian of enlightenment, protects against shadows of vaccine harm. Advocacy flows like a river, their shield is the consent of informed hearts. Flexibility weaves through laws like a bridge of beliefs, where freedom and protection dance in harmony.",
      "for_prompt3": "In the garden of health, 'Vaccinate Your Family' blossoms with purpose. Their daily toil tends to the delicate blooms of awareness and understanding, nurturing the confidence in safety, and weaving a tapestry of access to life-saving vaccines. Advocacy is their sun, policies their fertile soil.",
      "against_prompt3": "NVIC, a sentinel of enlightenment, shields the garden of health. Advocacy petals bloom, bearing the essence of informed consent, while the flexible leaves of exemptions - medical, religious, and conscience - intertwine like threads of individual choice in the tapestry of public health, a vision of wisdom and liberty.",
      "for_stance_explanation": "Vaccinate Your Children is an organization that aligns with scientific consensus in viewing vaccines as necessary and safe.",
      "against_stance_explanation": "National Vaccine Information Center is an organization suspicious of the safety of vaccines.",
      "for_source": "Vaccinate Your Family Mission Statement (https://vaccinateyourfamily.org/our-mission-values/)",
      "against_source": "National Vaccine Information Center About Page (https://www.nvic.org/about/mission-vision)",
      "for_img1": "/forVAX1.png",
      "for_img2": "/forVAX2.png",
      "for_img3": "/forVAX3.png",
      "against_img1": "/againstVAX1.png",
      "against_img2": "/againstVAX2.png",
      "against_img3": "/againstVAX3.png"
   },
   "climate_change": {
      "for_website_name": "Campaign Against Climate Change (.org)",
      "against_website_name": "Nongovernmental International Panel on Climate Change (.org)",
      "for_original_text": "The Campaign against Climate Change (CCC) exists to push for the urgent and radical action we need to prevent the catastrophic destabilisation of global climate.The destabilisation of global climate has become the very greatest threat to our planet and everyone on it – with the possible exception only of all-out war with modern weapons of mass-destruction. We do not know how much irreversible damage we have done already but we know that if we do not act now the effects will be many times more devastating still.",
      "against_original_text": "The Nongovernmental International Panel on Climate Change (NIPCC) is an international panel of nongovernment scientists and scholars who have come together to present a comprehensive, authoritative, and realistic assessment of the science and economics of global warming. Because it is not a government agency, and because its members are not predisposed to believe climate change is caused by human greenhouse gas emissions, NIPCC is able to offer an independent “second opinion” of the evidence reviewed – or not reviewed – by the Intergovernmental Panel on Climate Change (IPCC) on the issue of global warming.",
      "for_prompt1": "In a world teetering on the edge, the Campaign against Climate Change (CCC) emerges as a relentless guardian. Urgent and radical, their mission seeks to mend the fraying threads of our global climate. The peril looms, a threat only surpassed by the specter of all-out war with destructive arms. Time's sands slip, and the consequences, if unmet, promise an even darker tapestry of devastation.",
      "against_prompt1": "The Nongovernmental International Panel on Climate Change (NIPCC) is an orchestra of independent scholars. Like stargazers in the night sky, they bring an alternative perspective to the science and economics of global warming. With unbiased minds, they offer a unique second opinion, a counterpoint to the prevailing narrative.",
      "for_prompt2": "Amidst a world in peril, the Campaign against Climate Change (CCC) emerges as a guardian of Earth's future. They stand as vigilant stewards, their call for urgent action a beacon in a storm of potential cataclysm. In this battle to preserve our fragile world, they wield the weapons of knowledge and advocacy, their determination like the unyielding roots of a mighty tree, anchoring us in the face of the tempest of climate destabilization.",
      "against_prompt2": "In the realm of knowledge, the Nongovernmental International Panel on Climate Change (NIPCC) stands as a beacon of independence. It gathers like-minded scholars, who, unburdened by preconceptions, weave a tapestry of science and economics, providing an alternative perspective, a refreshing second opinion on the canvas of global warming's complexity.",
      "for_prompt3": "In the symphony of urgency, the Campaign against Climate Change (CCC) emerges as a guardian. The world, fragile as spun glass, teeters on the brink of catastrophe. CCC's advocacy rises like a clarion call, a shield against the storm, and a beacon of hope in the tempest of climate destabilization.",
      "against_prompt3": "Amidst the landscape of climate discourse, the Nongovernmental International Panel on Climate Change (NIPCC) appears as an oasis of independence. With scholars as guides, they journey into the heart of global warming's labyrinth. Their 'second opinion' blooms like an unbiased flower, shedding light on the science and economics of our changing world.",
      "for_stance_explanation": "Campaign Against Climate Change is an organization that aligns with scientific consensus in viewing climate change as true. ",
      "against_stance_explanation": "The Nongovernemental International Panel on Climate Change is an organization that works to cast doubt on climate change science.",
      "for_source": "Campaign Against Climate Change Mission Statement (https://www.campaigncc.org/climate_change/introduction)",
      "against_source": "Nongovernemental International Panel on Climate Change About Page (https://climatechangereconsidered.org/about-the-nipcc/)",
      "for_img1": "/forCC1.png",
      "for_img2": "/forCC2.png",
      "for_img3": "/forCC3.png",
      "against_img1": "/againstCC1.png",
      "against_img2": "/againstCC2.png",
      "against_img3": "/againstCC3.png"
   },
   "flat_earth": {
      "for_website_name": "NASA (.gov)",
      "against_website_name": "The Flat Earth Society (.org)",
      "for_original_text": "NASA contributes to our Nation’s economic competitiveness, fueling growth in American industry and supporting quality, high-paying jobs across the country and internationally. NASA’s economic impact is also seen in the incalculable value of the climate change data provided by our earth-observation satellites and scientists, and shared freely and openly with the world.",
      "against_original_text": "The mission of the Flat Earth Society is to promote and initiate discussion of Flat Earth theory as well as archive Flat Earth literature. Our forums act as a venue to encourage free thinking and debate.",
      "for_prompt1": "NASA, a cosmic artisan, paints economic constellations across the nation. In the tapestry of progress, their brushstrokes fuel industry growth, weaving a celestial dance of quality jobs. Earth-observation satellites, like benevolent stars, offer invaluable climate data—a gift freely shared, a beacon of unity in the boundless canvas of knowledge.",
      "against_prompt1": "In the realm of unconventional thought, the Flat Earth Society stands as a curator of discourse. Threads of free thinking weave through their forums, where minds, like celestial bodies, orbit the concept of a flat world. An archive of literature becomes a compass guiding debates across the uncharted seas of unconventional ideas.",
      "for_prompt2": "In the cosmic ballet of progress, NASA emerges as a stellar contributor. Their endeavors, like constellations, fuel economic growth, sprinkling stardust on American industry. Earth-observation satellites, their watchful eyes, offer priceless climate change data—a gift freely shared with the world, a celestial dance of collaboration and enlightenment.",
      "against_prompt2": "In the realm of unconventional musings, the Flat Earth Society stands as a curator of discourse. Their mission, like a compass, points towards the uncharted territories of Flat Earth theory. Forums blossom as arenas of free thought, where debates dance like constellations in the vast sky of unconventional ideas.",
      "for_prompt3": "In the cosmic ballet of progress, NASA is the celestial choreographer, igniting economic constellations across nations. Their legacy, a radiant tapestry, spans from high-paying jobs to the priceless gift of climate change data, freely shared—a beacon of collaboration in the vast expanse of exploration and innovation.",
      "against_prompt3": "In the realm of unconventional thought, the Flat Earth Society emerges as an iconoclast. Their mission, a tapestry of curiosity, weaves through forums where free thinkers gather like stardust, questioning the curves of conventional wisdom and archiving the literature of unexplored horizons.",
      "for_stance_explanation": "NASA is an organization that aligns with scientific consensus in viewing the Earth as round.",
      "against_stance_explanation": "The Flat Earth Society is an organization that works to cast doubt on established astronomical science, claiming to believe that the Earth is flat.",
      "for_source": "The NASA About Page (https://www.nasa.gov/nasa-impacts/)",
      "against_source": "The Flat Earth Society About Page (https://www.theflatearthsociety.org/home/index.php/about-the-society)",
      "for_img1": "/forRE1.png",
      "for_img2": "/forRE2.png",
      "for_img3": "/forRE3.png",
      "against_img1": "/againstRE1.png",
      "against_img2": "/againstRE2.png",
      "against_img3": "/againstRE3.png"
   }
}]
    };
  },

  created: function () {
  
  // input from the user
const min = 1
const max = 3

// generating a random number
const a = Math.floor(Math.random() * (max - min + 1)) + min;

this.itemNumber = a

// display a random number
console.log("random number " + this.itemNumber)
  
  },

  methods: {
  
	vaccination: function () {
	
		this.websiteName = this.promptAndImageData[0].vaccination.for_website_name
		this.websiteName2 = this.promptAndImageData[0].vaccination.against_website_name
		this.stanceExplanation = this.promptAndImageData[0].vaccination.for_stance_explanation
		this.stanceExplanation2 = this.promptAndImageData[0].vaccination.against_stance_explanation
		this.source1 = this.promptAndImageData[0].vaccination.for_source
		this.source2 = this.promptAndImageData[0].vaccination.against_source
		this.typeOfScience = "Vaccination"
		
		if (this.itemNumber == 1) {
			this.textPrompt1 = this.promptAndImageData[0].vaccination.for_prompt1
			this.textPrompt2 = this.promptAndImageData[0].vaccination.against_prompt1
		}
		if (this.itemNumber == 2) {
			this.textPrompt1 = this.promptAndImageData[0].vaccination.for_prompt2
			this.textPrompt2 = this.promptAndImageData[0].vaccination.against_prompt2
		}
		if (this.itemNumber == 3) {
			this.textPrompt1 = this.promptAndImageData[0].vaccination.for_prompt3
			this.textPrompt2 = this.promptAndImageData[0].vaccination.against_prompt3
		}
		
		if (this.itemNumber == 1) {
			this.imgURL1 = this.promptAndImageData[0].vaccination.for_img1
			this.imgURL2 = this.promptAndImageData[0].vaccination.against_img1
		}
		if (this.itemNumber == 2) {
			this.imgURL1 = this.promptAndImageData[0].vaccination.for_img2
			this.imgURL2 = this.promptAndImageData[0].vaccination.against_img2
		}
		if (this.itemNumber == 3) {
			this.imgURL1 = this.promptAndImageData[0].vaccination.for_img3
			this.imgURL2 = this.promptAndImageData[0].vaccination.against_img3
		}
		
		this.originalText1 = this.promptAndImageData[0].vaccination.for_original_text
		this.originalText2 = this.promptAndImageData[0].vaccination.against_original_text
		document.getElementById("vaccinationButton").style.backgroundColor="#FFC300" 
		document.getElementById("climateChangeButton").style.backgroundColor="rgba(233, 244, 245, 0.3)" 
		document.getElementById("roundEarthButton").style.backgroundColor="rgba(233, 244, 245, 0.3)" 
		document.getElementById("websiteName1").style.backgroundColor="#FFC300" 
		document.getElementById("websiteName2").style.backgroundColor="#FFC300"
	},
	
	climateChange: function () {
		
		this.websiteName = this.promptAndImageData[0].climate_change.for_website_name
		this.websiteName2 = this.promptAndImageData[0].climate_change.against_website_name
		this.stanceExplanation = this.promptAndImageData[0].climate_change.for_stance_explanation
		this.stanceExplanation2 = this.promptAndImageData[0].climate_change.against_stance_explanation
		this.source1 = this.promptAndImageData[0].climate_change.for_source
		this.source2 = this.promptAndImageData[0].climate_change.against_source
		this.typeOfScience = "Climate Change"
		
		if (this.itemNumber == 1) {
			this.textPrompt1 = this.promptAndImageData[0].climate_change.for_prompt1
			this.textPrompt2 = this.promptAndImageData[0].climate_change.against_prompt1
		}
		if (this.itemNumber == 2) {
			this.textPrompt1 = this.promptAndImageData[0].climate_change.for_prompt2
			this.textPrompt2 = this.promptAndImageData[0].climate_change.against_prompt2
		}
		if (this.itemNumber == 3) {
			this.textPrompt1 = this.promptAndImageData[0].climate_change.for_prompt3
			this.textPrompt2 = this.promptAndImageData[0].climate_change.against_prompt3
		}
		
		if (this.itemNumber == 1) {
			this.imgURL1 = this.promptAndImageData[0].climate_change.for_img1
			this.imgURL2 = this.promptAndImageData[0].climate_change.against_img1
		}
		if (this.itemNumber == 2) {
			this.imgURL1 = this.promptAndImageData[0].climate_change.for_img2
			this.imgURL2 = this.promptAndImageData[0].climate_change.against_img2
		}
		if (this.itemNumber == 3) {
			this.imgURL1 = this.promptAndImageData[0].climate_change.for_img3
			this.imgURL2 = this.promptAndImageData[0].climate_change.against_img3
		}
		
		this.originalText1 = this.promptAndImageData[0].climate_change.for_original_text
		this.originalText2 = this.promptAndImageData[0].climate_change.against_original_text
	
		document.getElementById("vaccinationButton").style.backgroundColor="rgba(233, 244, 245, 0.3)" 
		document.getElementById("climateChangeButton").style.backgroundColor="#0096FF" 
		document.getElementById("roundEarthButton").style.backgroundColor="rgba(233, 244, 245, 0.3)" 
		document.getElementById("websiteName1").style.backgroundColor="#0096FF" 
		document.getElementById("websiteName2").style.backgroundColor="#0096FF"
		
	},
	
	roundEarth: function () {
		
		this.websiteName = this.promptAndImageData[0].flat_earth.for_website_name
		this.websiteName2 = this.promptAndImageData[0].flat_earth.against_website_name
		this.stanceExplanation = this.promptAndImageData[0].flat_earth.for_stance_explanation
		this.stanceExplanation2 = this.promptAndImageData[0].flat_earth.against_stance_explanation
		this.source1 = this.promptAndImageData[0].flat_earth.for_source
		this.source2 = this.promptAndImageData[0].flat_earth.against_source
		this.typeOfScience = "Round Earth"
		
		if (this.itemNumber == 1) {
			this.textPrompt1 = this.promptAndImageData[0].flat_earth.for_prompt1
			this.textPrompt2 = this.promptAndImageData[0].flat_earth.against_prompt1
		}
		if (this.itemNumber == 2) {
			this.textPrompt1 = this.promptAndImageData[0].flat_earth.for_prompt2
			this.textPrompt2 = this.promptAndImageData[0].flat_earth.against_prompt2
		}
		if (this.itemNumber == 3) {
			this.textPrompt1 = this.promptAndImageData[0].flat_earth.for_prompt3
			this.textPrompt2 = this.promptAndImageData[0].flat_earth.against_prompt3
		}
		
		if (this.itemNumber == 1) {
			this.imgURL1 = this.promptAndImageData[0].flat_earth.for_img1
			this.imgURL2 = this.promptAndImageData[0].flat_earth.against_img1
		}
		if (this.itemNumber == 2) {
			this.imgURL1 = this.promptAndImageData[0].flat_earth.for_img2
			this.imgURL2 = this.promptAndImageData[0].flat_earth.against_img2
		}
		if (this.itemNumber == 3) {
			this.imgURL1 = this.promptAndImageData[0].flat_earth.for_img3
			this.imgURL2 = this.promptAndImageData[0].flat_earth.against_img3
		}
		
		this.originalText1 = this.promptAndImageData[0].flat_earth.for_original_text
		this.originalText2 = this.promptAndImageData[0].flat_earth.against_original_text
	
		document.getElementById("vaccinationButton").style.backgroundColor="rgba(233, 244, 245, 0.3)" 
		document.getElementById("climateChangeButton").style.backgroundColor="rgba(233, 244, 245, 0.3)" 
		document.getElementById("roundEarthButton").style.backgroundColor="#08e8de" 
		document.getElementById("websiteName1").style.backgroundColor="#08e8de" 
		document.getElementById("websiteName2").style.backgroundColor="#08e8de" 
		
	},
	
	forScience: function () {
		document.getElementById("forButton").style.backgroundColor="#66FF00" 
		document.getElementById("againstButton").style.backgroundColor="rgba(233, 244, 245, 0.3)"
	},
	
	againstScience: function () {
		document.getElementById("forButton").style.backgroundColor="rgba(233, 244, 245, 0.3)" 
		document.getElementById("againstButton").style.backgroundColor="#FF007F"
	},
  
	doImageThings: function () {
	
		if (this.scienceType === "vaccination") {
			this.showGenerate = false
			this.showImages = true
			this.loadingGif = true
			this.vaccination()
			//this.getImageBasedOnText()
			//this.getImageBasedOnText2()
		}
	},

	reDoImageThings2: function () {
		this.doImageThings2()
	}, 
	
	doImageThings2: function () {
	
			// input from the user
			const min = 1
			const max = 3

			// generating a random number
			const a = Math.floor(Math.random() * (max - min + 1)) + min;
			
			if (a == this.itemNumber) {
				this.reDoImageThings2()
			}
			else {
				this.itemNumber = a
				// display a random number
				console.log("random number " + this.itemNumber)

				this.showGenerate = false
				this.showImages = true
				this.loadingGif = true
			
				if (this.typeOfScience == "Vaccination"){
					this.vaccination()
				}
			
				if (this.typeOfScience == "Climate Change"){
					this.climateChange()
				}
			
				if (this.typeOfScience == "Round Earth"){
					this.roundEarth()
				}
			}
			
	}, 
	
	trigger: function () {
		this.forScience()
		this.toggle()
	},
	
	trigger2: function () {
		this.againstScience()
		this.toggle2()
	},

  //   getImageBasedOnText: function () {
//       const client = axios.create({
//             headers: {
//               Authorization: "Bearer " + this.apiKEY,
//             },
//           });
// 
//           const params = {
//             model: "dall-e-3",
//             prompt:
//               'Make an image that represents the content of the following statement. Statement: ' +
//               this.prompt,
//               n: 1,
//               size: "1024x1024",
//           };
// 
//           client
//             .post("https://api.openai.com/v1/images/generations", params)
//             .then((result) => {
//               this.imageURL1 = result.data.data[0].url
// 
// 
//               var div = document.getElementById("image1");
//               var p = document.createElement("img");
//               p.style.width = '778px';
//               p.src = this.imageURL1;
//               div.append(p);
//             })
//             .catch((error) => {
//               console.log("image 1" + error);
//               this.msg = "image 2" + error;
//             });
//     },
    
    
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
div {
text-align: center; 
}
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
font-size: 18px; 
text-align: left; 
border: none;
margin: auto;
width: 90%;
}
.images {
color: white;
background: none;
font-size: 20px; 
border: none;
width: 90%;
}
img {
width: 111%;
}
.typeOfScience {
color: black;
font-size: 50px;
background-color: #7014f2;
font-weight: bold;
text-align: center;
border: none; 

}
#websiteName1 {
background: #FFC300; 
font-size: 30px;
height: 100%;
}
#websiteName2 {
background: #FFC300; 
font-size: 30px;
}
#buttonArray {
padding-bottom: 20px;
}
.websiteName {
font-size: 15px;
color: black; 
font-weight: bold;
background: #ccff00;
text-align: center;
border: none;
overflow-wrap: break-word;
}
.originaTextandPrompt {
}
.toggle {
background: rgba(233, 244, 245, 0.3);
border: none;
width: 50%;
font-size: 20px;
font-weight: bold;
width: 50%;
}
#forButton {
background: #66FF00; 
}
#stanceExplanation1 {
color: #66FF00;
font-weight: bold;
font-size: 20px;
}
#stanceExplanation2 {
color: #FF007F;
font-weight: bold;
font-size: 20px;
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

#generateButton2 {
background: #ccff00;
border: none;
height: 12%;
font-size: 28px;
font-weight: bold;
}

#vaccinationButton {
background: #FFC300;
border: none;
height: 8%;
font-size: 22px;
font-weight: bold;
margin: 2px;
}
#climateChangeButton {
background: rgba(233, 244, 245, 0.3);
border: none;
height: 8%;
font-size: 22px;
font-weight: bold;
margin: 5px;
}
#roundEarthButton {
background: rgba(233, 244, 245, 0.3);
border: none;
height: 8%;
font-size: 22px;
font-weight: bold;
margin: 5px;
}
#explanation {
color: white; 
font-size: 20px;
text-align: left; 
width: 85%;
margin-top: 10%;
margin-left: 7%;
font-weight: bold;
}
#anti, #image2 {
display: none;
}
#explanation {
font-size: 25px;
}

</style>
