<template lang="pug">
.poster
  .frame(:class="[activeId]")
  .link(
    v-for="(item, key) in list"
    @click="handleActiveId(item.id)"
    :class="item.id"
    :key="key"
  )
.section-wrapper
  section(
    v-for="(item, key) in list"
    :ref="setSectionRef"
    :id="item.id"
    :key="key"
  )
    .name
      span(v-for="(name, key) in item.character.split(' ')", :key="key") {{ name }}
    .character-img(:style="{backgroundImage: `url(${item.img})`}")
    .desc(v-html="item.desc")
</template>

<script>
import { defineComponent, ref } from 'vue'

const data = [
  {
    id: 'ross',
    character: 'Ross Geller',
    img: 'https://static.wikia.nocookie.net/friends/images/0/0b/RossGeller.jpg',
    desc: "Ross Geller (David Schwimmer) is a paleontologist at a museum of prehistory, and later a professor of paleontology at New York University. Considered by some to be the most intelligent of the six main characters, but at the same time a clumsy, quirky man, Ross is known for being a smart, know-it-all who prides himself on his rationality, despite his clear hopeless romanticism. He is shown to be the most caring of all six members in various instances on the show, but also shown to be the most neurotic and his insecurities often gets him in trouble. \n Ross is Monica's older brother, Chandler's college roommate, and Rachel's on-again, off-again boyfriend. His first marriage has already failed by the time the show begins, with his second lasting mere weeks. The second divorce seemed to greatly depress him and make him quick-tempered near the start of season five, such as when he screams at his colleague for taking his sandwich and throwing it away. After he gets Ugly Naked Guy's apartment, he is offered to return to work, but he loses his temper again when he sees Chandler and Monica having sex through his window, becoming the last one to find out about their relationship. He also drunkenly married Rachel in Las Vegas, after which they unsuccessfully tried to annul it and had to settle for a divorce, which became Ross' third one. Ross's relationship with Rachel is a major storyline throughout the series. He is also the father of his ex-wife Carol's son, Ben, and Rachel's daughter, Emma. In the series finale, Ross and Rachel finally reconcile, deciding to be together once and for all. \n The character of Ross was developed with David Schwimmer in the minds of writers and Schwimmer was also the first actor to be cast on the show. \n Before being cast in Friends, Schwimmer played minor characters in The Wonder Years and NYPD Blue; his first regular series role was in the sitcom Monty. Schwimmer is the only cast member native to New York City. \n Ross Geller was also voted as the best character on Friends in a poll conducted by Comedy Central UK.",
  },
  {
    id: 'rachel',
    character: 'Rachel Green',
    img: 'https://static.wikia.nocookie.net/friends/images/f/f7/Rachel_Greene.jpg',
    desc: "Rachel Karen Green (Jennifer Aniston) is the spoiled but warm-hearted and likable daughter of Dr. Leonard Green (Ron Leibman), a rich, Long Island vascular surgeon, and Sandra Green (Marlo Thomas). She has two sisters, Jill (Reese Witherspoon) and Amy (Christina Applegate). Rachel is introduced into the series in the first episode after she leaves her fiancé, Barry, at the altar, and attempts to live independently without financial support from her parents. She flees from her almost-wedding to New York City to find Monica Geller, her friend from high school. Rachel moves into Monica's apartment and meets Phoebe Buffay and Joey Tribbiani. Rachel already knows Ross Geller, Monica's brother, as all three attended Lincoln High School. In the first episode, she is also reacquainted with Chandler Bing, Ross's college buddy; however, later episodes retcon this, and she is shown to have met Chandler on Thanksgiving, whilst Ross was at college. Rachel's first job is as a waitress at Central Perk coffee house. She later begins to work in fashion, becoming an assistant buyer, and later a personal shopper, at Bloomingdale's. She eventually becomes a buyer at Polo Ralph Lauren.\n A great deal of Rachel's life throughout the series revolves around her relationship with Ross Geller. At the end of season seven, during Monica's and Chandler's wedding, it is revealed that Rachel is pregnant from a one-night stand with Ross. Initially, Rachel is determined to raise the baby on her own, but later she realizes she needs Ross's help. She decides to move in with Ross, even though the two are not involved in an intimate relationship. Their daughter is born during the eighth-season finale. Her aunt Monica, 'gives' an indecisive Rachel the name Emma, which she had chosen for her own daughter, at age 14.\n During the tenth season, Rachel is offered a job with Louis Vuitton in Paris. She accepts and prepares to move herself and Emma to France. However, in the series finale, she declines the job offer and famously 'gets off the plane'. Rachel and Ross get back together in the final episode of the series.",
  },
  {
    id: 'monica',
    character: 'Monica Geller',
    img: 'https://static.wikia.nocookie.net/friends/images/2/2f/Monica_Geller-Bing_Season_10.png',
    desc: "Monica E. Geller (Courteney Cox) is the younger sister of Ross and best friend of Rachel, the latter of whom she allows to live with her after Rachel forsakes her own wedding. She works primarily as a chef at a variety of restaurants. She is described as the mother hen of the group, and is known for her obsessive-compulsive and competitive nature.[19][20] Monica is often jokingly teased by the others, especially Ross, for having been overweight as a teen. \n In the second season, Monica falls for her father Jack's (Elliott Gould) friend, Richard Burke (Tom Selleck). Despite the twenty-one year age difference, Monica and Richard are happy, and her parents accept their relationship. However, as a result of Monica yearning for a family but Richard having already had one, they break up at the end of the second season. Monica then pursues a chain of various men until she unexpectedly begins a relationship with her longtime friend, Chandler Bing, at the end of the fourth season, during her brother Ross' wedding to Emily Waltham in London. Monica and Chandler try to hide their relationship from the rest of the group for much of the fifth season, but eventually everyone finds out. After celebrating their first anniversary in Las Vegas, they move in together and get engaged by the sixth-season finale. After their marriage, Monica and Chandler try to conceive children, only to discover that they are unable to do so. In the final season of the series, they adopt new-born twins, whom they name Erica and Jack.",
  },
  {
    id: 'chandler',
    character: 'Chandler Bing',
    img: 'https://static.wikia.nocookie.net/friends/images/4/48/Chandler_Bing_portrait.jpg',
    desc: "Chandler Muriel Bing (Matthew Perry) is an executive in statistical analysis and data reconfiguration for a large multi-national corporation. He later quits his job and becomes a junior copywriter at an advertising agency. \n Chandler is known for his sarcastic sense of humor. He often teases his best friend Joey for the latter's stupidity. Chandler is often depicted as being somewhat of a hapless individual, suffering a lot of bad luck while struggling through life and occasionally struggling with an on-and-off smoking addiction. However, he eventually falls in deep mutual love with Monica and proposes to her at the close of season six, with the two of them marrying at the close of season seven. At the end of series, he and Monica adopt twins, whom they name Jack and Erica. \n Like Aniston and LeBlanc, Perry had already appeared in several unsuccessful sitcom pilots before being cast. He had also starred in the TV series Second Chance and Sydney.",
  },
  {
    id: 'phoebe',
    character: 'Phoebe Buffay',
    img: 'https://static.wikia.nocookie.net/friends/images/3/30/PhoebeBuffay.jpg',
    desc: "Phoebe Buffay-Hannigan (Lisa Kudrow) is an odd, ditzy albeit sweet-natured masseuse who grew up homeless, sometimes telling her friends outlandish tales of life on the street. She is an aspiring musician who plays the guitar and sings songs with somewhat unusual lyrics at the coffee shop. She has an identical twin sister, Ursula Pamela Buffay (also played by Kudrow), who is just as odd as Phoebe and appeared as a recurring character on Mad About You. After a series of dates and relationships with a number of men, Phoebe meets Mike Hannigan (Paul Rudd) in season nine, whom she eventually marries in season ten. She also became a surrogate mother for her half-brother Frank Jr. (Giovanni Ribisi), giving birth to his triplets in the fifth season.",
  },
  {
    id: 'joey',
    character: 'Joey Tribbiani',
    img: 'https://static.wikia.nocookie.net/friends/images/f/f5/JoeyTribbiani.jpg',
    desc: "Joseph Francis 'Joey' Tribbiani Jr. (Matt LeBlanc) is a good-natured but not-so-bright struggling actor and food lover, who becomes mildly famous for his role as Dr. Drake Ramoray on a fictionalized version of Days of Our Lives. Joey is a womanizer, with many girlfriends throughout the series, often using his catchphrase pick-up line 'How you doin'?'' He develops a crush on Rachel in season eight. \n Prior to his role on Friends, LeBlanc appeared as a regular on the short-lived TV 101, a minor character in the sitcom Married... with Children, and as a main character in its spin-offs, Top of the Heap and Vinnie & Bobby. After Friends ended, LeBlanc portrayed Joey in a short-lived spin-off, Joey.",
  },
]

export default defineComponent({
  name: 'App',
  setup() {
    const list = data.map(el => {
      let desc = el.desc
        .replaceAll('\n', '<br>')
        .replaceAll('(', '<small>(')
        .replaceAll(')', ')</small>')
      return { ...el, desc }
    })

    const activeId = ref(list[0].id)

    const onElView = entries => {
      for (const entry of entries) {
        if (entry.isIntersecting) {
          activeId.value = entry.target.id
        }
      }
    }

    const options = {
      root: document.querySelector('.section-wrapper'),
      rootMargin: '-50% 0% -50%',
    }

    const watcher = new IntersectionObserver(onElView, options)
    const sectionRefs = ref([])

    const setSectionRef = el => {
      if (el) {
        sectionRefs.value.push(el)
        watcher.observe(el)
      }
    }

    const handleActiveId = (id) => {
      const index = list.findIndex(el => el.id === id)
      sectionRefs.value[index].scrollIntoView({ behavior: 'smooth' })
    }

    return {
      activeId,
      handleActiveId,
      setSectionRef,
      list
    }
  }
})
</script>

<!--suppress CssUnknownTarget -->
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@300;400;800&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Kalam&display=swap');


@mixin flexCenter {
  display: flex;
  justify-content: center;
  align-items: center;
}

* {
  box-sizing: border-box;
}

html {
  font-size: 100vmax / 1600 * 100;
  @media (max-width: 992px) {
    font-size: 60px;
  }
}

body {
  height: 100vh;
  @include flexCenter;
  background-color: #fcfcfc;
  overflow: hidden;
  padding: .2rem;
  font-size: .3rem;
  font-family: 'Nunito', sans-serif;
  position: relative;
  &:before {
    content: '';
    position: absolute;
    bottom: 0;
    right: 0;
    width: 2.7rem;
    height: 1.2rem;
    background: url(https://logos-world.net/wp-content/uploads/2021/08/Friends-Logo.png) no-repeat center / contain;
    opacity: 0.7;
    z-index: -1;
  }
}

#app {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

$section-w: 6rem;
$frame-size: .8rem;

.poster {
  position: relative;
  width: $section-w;
  padding-top: $section-w * .2;
  margin: auto;
  background: url(https://www.namechef.co/files/ck/images/friends-sitcom.jpeg) no-repeat center top / 100% auto;
  border-radius: 0.1rem;
  overflow: hidden;
  z-index: 1;
  .frame {
    position: absolute;
    top: -$frame-size;
    left: -$frame-size;
    width: $frame-size;
    height: $frame-size;
    border-radius: 50%;
    transition: all 0.2s;
    box-shadow: 0 0 0 10rem rgba(#000, .7);
  }
  .link {
    position: absolute;
    width: $frame-size;
    height: $frame-size;
    border-radius: 50%;
    cursor: pointer;
  }
  .ross {
    top: 2%;
    left: 2.6%;
  }
  .rachel {
    top: 30%;
    left: 13.5%;
  }
  .monica {
    top: 25%;
    left: 33%;
  }
  .chandler {
    top: 2%;
    left: 49%;
  }
  .phoebe {
    top: 8%;
    left: 67%;
  }
  .joey {
    top: 9%;
    left: 85%;
  }
}

.section-wrapper {
  width: 100%;
  max-width: 10rem;
  flex-grow: 1;
  margin-top: 0.1rem;
  overflow: auto;
}

section {
  margin: 0.2rem auto 0.6rem;
  padding: 0 1rem;
}

.name {
  text-align: center;
  font-family: 'Kalam', cursive;
  text-transform: uppercase;
  margin-bottom: 0.15rem;
  &:before {
    content: '\2022';
    color: #e11d22;
    font-size: 1.3em;
    margin-right: 0.05rem;
  }
  &:after {
    content: '\2022';
    color: #0dace0;
    font-size: 1.3em;
    margin-left: 0.05rem;
  }
  span:first-child:after {
    content: '\2022';
    color: #f3b715;
    font-size: 1.3em;
    margin: 0 0.05rem;
  }
}

.character-img {
  margin: 0 auto 0.1rem;
  width: 2rem;
  height: 2rem;
  background-size: 120% auto;
  background-position: center top;
  border-radius: 0.3rem;
  box-shadow: 3px 3px 10px #777;
}

.desc {
  font-weight: lighter;
  font-size: .2rem;
  line-height: 1.8;
  text-shadow: 0 0 2px #fff, 0 0 4px #fff, 0 0 6px #fff;
  br {
    content: '';
    display: block;
    margin-bottom: .1rem;
  }
  small {
    font-style: italic;
  }
}
</style>
