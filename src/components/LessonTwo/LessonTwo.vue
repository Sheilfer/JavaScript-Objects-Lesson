<template>
  <div id="app">
    <b-container fluid>
      <h3 class="text-center"><img src="https://codemoji.com/images/white-logo.png" class="codemoji-logo"> Lesson 2 of 20</h3>
      <b-progress height="20px" :value="value" :max="max"></b-progress>
    </b-container>
    <br>

    <b-container fluid>
      <b-row>

        <b-col class="col-md-3 d-flex align-items-stretch">

            <b-card header="What is a Boolean?"
                header-tag="header">
              <b-form-textarea plaintext style="width: 289px; height: 515px;" value="You made your very first variable! Awesome! You’re probably wondering where your pizza is! Maybe you were expecting it because we created our pizza variable. So why didn’t it show up?

It didn’t show up because while our variable has a name, it is empty. Variables can be filled with all sorts of information. In this lesson, we’re going to fill our variable with a Boolean

A boolean is a special type of value that is either true or false. We’re going to give our variable the value of true to make our pizza appear!

Set your variable to true by typing this: var pizza = true; "></b-form-textarea>
        </b-card>
          </b-col>

          <b-col class ="col-md-5 d-flex align-items-stretch">
            <b-card header="Chef Leo is here to help! Follow his instructions below!"
                header-tag="header">
              <b-container>
                <b-row>
                  <b-col class="col-md-3"><img class="chef" src="../../../cartoonchef.png"></b-col>
                  <b-col class="col-md-9">
                    <b-list-group>
                      <b-list-group-item> 1: Set var pizza to true, like this! var pizza = true;</b-list-group-item>
                    </b-list-group>
                  </b-col>
                </b-row>
              </b-container>
                  <div class="codemirror">
                    <codemirror :value="code" v-model="code" :options="cmOption"></codemirror>
                  </div>
              <div class="card-footer">
                <button class="btn btn-success btn-block" @click="checkAnswer">Ok! Check my code!</button>
              </div>
            </b-card>
          </b-col>

          <b-col class="col-md-4 d-flex align-items-stretch">
            <b-card header="Chef's Table"
                header-tag="header">
            <div class="text-center parent">

                <transition name="fade" mode="out-in">
                  <img v-if="pizza" class="pizza" src="../../../pizza.jpg">
                </transition>

              <img class="cuttingboard" src="../../../cuttingboard.png">
              </div>
              <div class="card-footer">
                <b-btn v-if="pizza" @click="changeLesson" variant="success" class="btn-block">Next lesson</b-btn>
              </div>
            </b-card>
          </b-col>

      </b-row>
    </b-container>

    <br>

<div>
  <b-modal v-model ="showLessonDetails" hide-footer title="Codemoji JavaScript">
      <div class="d-block text-center">
        <h4>Lesson 2: What is a Boolean? </h4>
      </div>
      <p>You made your very first variable! Awesome! You’re probably wondering where your pizza is! Maybe you were expecting it because we created our pizza variable. So why didn’t it show up?
<br><br>
It didn’t show up because while our variable has a name, it is empty. Variables can be filled with all sorts of information. In this lesson, we’re going to fill our variable with a Boolean
<br><br>
A boolean is a special type of value that is either true or false. We’re going to give our variable the value of true to make our pizza appear!
<br><br>
Set your variable to true by typing this: var pizza = true; </p>
      <b-btn class="mt-3" variant="success" block @click="showLessonDetails=!showLessonDetails">Ok, got it!</b-btn>
    </b-modal>
    <b-modal ref="myModalRef" hide-footer title="Codemoji JavaScript">
      <div class="d-block text-center">
        <h4>Great job! </h4>
      </div>
      <b-btn class="mt-3" variant="success" block @click="showPizza">Submit and show animation</b-btn>
    </b-modal>

    <b-modal ref="myErrorRef" hide-footer title="Codemoji Objects">
      <div class="d-block text-center">
        <h4>Hmm, that's not quite right</h4>
      </div>
      <p>{{ errorMessage }}</p>
      <b-btn class="mt-3" variant="danger" block @click="hideError">Let me try again!</b-btn>
    </b-modal>

  </div>
  </div>
</template>

<script>
  // language
  import 'codemirror/mode/javascript/javascript.js'
  // require active-line.js
  import'codemirror/addon/selection/active-line.js'
  import'codemirror/mode/clike/clike.js'
  import'codemirror/addon/comment/comment.js'
import Output from "./Output";
export default {
  props:['lessonTwo'],
  data() {
    return {
      code: ``,
      showAlert: false,
      showOnloadModal: true,
      answer: `varpizza=true;`,
      value: 10,
      max: 100,
      showLessonDetails: true,
      errorMessage: '',
      pizza: false,
      cmOption: {
        styleActiveLine: false,
        lineNumbers: true,
        line: true,
      }
    }
  },
  components: {
    appOutput: Output
  },
  methods: {
    showPizza () {
      this.pizza = true;
      this.$refs.myModalRef.hide();
    },
    changeLesson () {
      this.$emit('lessonChanged')
    },
    showModal () {
      this.$refs.myModalRef.show()
    },
    hideModal () {
      this.$refs.myModalRef.hide()
    },
    showError() {
      this.$refs.myErrorRef.show();
    },
    hideError() {
      this.$refs.myErrorRef.hide();
    },
    checkAnswer() {
      let myAnswer = this.code.toLowerCase().split('');
      //removes spaces from code so that students won't get errors for spacing
      for(let i = 0; i < myAnswer.length; i++) {
        if (myAnswer[i] == ' ') {
          myAnswer.splice(i, 1);
        }
      }

      if(myAnswer.length> 13) {
        this.errorMessage = "Looks like you missed a few things. Make sure your code looks like this: var pizza = true;"
      } else if (myAnswer[0]+myAnswer[1]+myAnswer[2] !== 'var') {
        this.errorMessage = "Looks like you either forgot to type 'var' or misspelled it, go back and try again!";
      } else if(myAnswer[3]+myAnswer[4]+myAnswer[5]+myAnswer[6]+myAnswer[7] !== 'pizza') {
        this.errorMessage = "Looks like you forgot to type 'pizza' or misspelled it, go back and try again!";
      } else if (myAnswer[8]!=='=') {
        this.errorMessage = "Looks like you missed your equals sign after pizza. Place one there and try again!"
      } else if (myAnswer[9]+myAnswer[10]+myAnswer[11]+myAnswer[12] !== "true") {
        this.errorMessage = "Looks like you forgot to type 'true' or misspelled it, try again!"
      } else if(myAnswer[13]!==';') {
        this.errorMessage = "Whoops, looks like you missed a semicolon (;), make sure to add one after the word 'pizza'!"
      } else if(myAnswer!== this.code) {
        this.errorMessage = "Looks like you missed a few things. Make sure your code looks like this: var pizza = true;"
      }

      myAnswer= myAnswer.join('');

      if(myAnswer == this.answer) {
        this.showModal();
      } else if (myAnswer !== this.answer){
        this.showError();
      }
    }
  }

}
</script>

<style>

.parent {
  position: relative;
  top: 0;
  left: 0;
}


.cuttingboard {
  width: 93%;
  position: relative;
  top: 0;
  left: 0;
  z-index: 0;
}

.pizza {
  position: absolute;
  margin-top: 70px;
  margin-left: 60px;
  top: 0;
  left: 0;
  z-index: 1;
  width: 60%;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.fade-enter {
        opacity: 0;
    }
    .fade-enter-active {
        transition: opacity 0.5s;
    }
    .fade-leave-active {
        transition: opactiy 0.5s;
        opacity: 0;
    }

.CodeMirror-focused .cm-matchhighlight {
  background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAIAAAACCAYAAABytg0kAAAAFklEQVQI12NgYGBgkKzc8x9CMDAwAAAmhwSbidEoSQAAAABJRU5ErkJggg==);
  background-position: bottom;
  background-repeat: repeat-x;
}
.cm-matchhighlight {background-color: lightgreen}
.CodeMirror-selection-highlight-scrollbar {background-color: green}
.chef {
  width: 75%;
}
</style>
