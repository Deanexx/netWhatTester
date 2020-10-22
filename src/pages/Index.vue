<template>
  <q-page class="flex flex-center">
    <div class="fixed-bottom-right bg-deep-purple-6 text-center"
         style="width: 50px; border-radius: 10%; z-index: 999">
        {{ minutes}} : {{ seconds }}
    </div>
    <transition enter-active-class="animated fadeIn"
                leave-active-class="animated fadeOut"
                mode="out-in"
                :duration="300">
      <div class="test"
           :key="1"
           v-if="test">
        <q-stepper
          animated
          v-model="step"
          ref="stepper"
          vertical
        >
          <q-step
            v-for="(item, i) in quiz"
            :key="i"
            :title="item.title"
            :name="i + 1"
            :done="item.result === true"
            :error="item.result === false"
            done-color="green"
            active-color="deep-orange"
          >
            <div class="q-gutter-sm
              column
              rounded-borders
              styling__radio">
              <q-radio
                color="deep-orange"
                v-for="(question, j) in item.questions"
                :key="j"
                v-model="item.answer"
                :val="j + 1"
                :label="question"/>
            </div>
            <q-stepper-navigation>
              <q-btn
                v-if="step < quiz.length"
                @click="checkAnswer(i)"
                color="deep-orange">
                Next
              </q-btn>
              <q-btn
                :loading="loading"
                v-if="step >= quiz.length"
                @click="checkAnswer(i); showResult()"
                color="green">
                Get result
              </q-btn>
            </q-stepper-navigation>
          </q-step>
        </q-stepper>
      </div>
      <div
        class="result"
        :key="2"
        v-if="!test"
      >
        <div v-if="resultArr.length === 0">
          <p class="text-h3 bg-green">Look at you! You did no mistake!</p>
        </div>
        <div v-else>
          <p class="text-h4 text-center">You did mistakes in those questions</p>
          <q-card
            v-for="(result, i) in this.resultArr"
            class="q-mb-md"
            :key="i">
            <q-card-section
              class="text-center"
              style="background: lightgoldenrodyellow">
              {{ result.title }}
            </q-card-section>
            <q-card-section style="background: lightcoral">
              <p class="flex justify-center">Your answer:</p>
              <p>{{ !result.answer ? "You didn't select answer" : result.questions[result.answer - 1] }}</p>
            </q-card-section>
            <q-card-section style="background: lightgreen">
              <p class="flex justify-center">Right answer:</p>
              <p>{{ result.questions[result.rightAnswer - 1] }}</p>
            </q-card-section>
          </q-card>
        </div>
      </div>
    </transition>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data(){
    return {
      interval: null,
      minutes: 10,
      seconds: 0,
      loading: false,
      step: 1,
      test: true,
      resultArr: [],
      quiz: [
        {
          title: "Which of the following proposals is the valid host range for the subnet on which the IP address 158.167.18.156/15 resides?",
          questions: [
            "158.166.0.1- 158.167.255.253",
            "158.165.255.253- 158.167.255.254",
            "158.166.0.1- 158.167.255.254",
            "158.166.0.2- 158.168.0.2"
          ],
          answer: null,
          rightAnswer: 3,
          result: null
        },
        {
          title: "If the IP address 123.48.189.194/21 is assigned to an Ethernet port of a router, what host address could communicate with it?",
          questions: [
            "101.219.223.235",
            "75.153.38.143",
            "5.200.165.154",
            "13.28.168.153",
            "172.1.223.196",
            "43.241.96.42",
            "123.48.189.109",
            "253.99.227.186"
          ],
          answer: null,
          rightAnswer: 7,
          result: null
        },
        {
          title: "Which of the following proposals is a private IP address?",
          questions: [
            "57.195.242.245",
            "172.27.217.52",
            "249.204.256.26",
            "249.204.156.26",
            "4.137.228.63",
            "176.37.230.43",
            "218.106.207.158"
          ],
          answer: null,
          rightAnswer: 2,
          result: null
        },
        {
          title: "What is the network address of a host with an IP address of 182.161.121.118/24?",
          questions: [
            "180.0.0.0",
            "182.161.121.64",
            "182.161.120.0",
            "182.161.121.116",
            "0.0.0.0",
            "182.161.96.0",
            "182.160.0.0",
            "182.161.121.0"
          ],
          answer: null,
          rightAnswer: 8,
          result: null
        },
        {
          title: "What is the maximum number of IP addresses that can be assigned to hosts on a local subnet using the 255.255.128.0 subnet mask?",
          questions: [
            "65536",
            "65532",
            "16380",
            "32768",
            "32770",
            "65530",
            "32766",
            "16382",
            "32764"
          ],
          answer: null,
          rightAnswer: 7,
          result: null
        },
        {
          title: "You want to implement a mechanism that automates IP configuration, including IP address, subnet mask, default gateway and DNS information. What protocol will you use to achieve this?",
          questions: [
            "SNMP",
            "DHCP",
            "SMTP",
            "ARP"
          ],
          answer: null,
          rightAnswer: 2,
          result: null
        },
        {
          title: "You have an interface on a router with the IP address of 124.144.156.248/21. Including the router interface, how many hosts can have IP addresses on the local network connected to the router interface?",
          questions: [
            "1020",
            "2050",
            "2044",
            "4090",
            "2046",
            "2048",
            "4092",
            "2042",
            "4094"
          ],
          answer: null,
          rightAnswer: 5,
          result: null
        },
        {
          title: "Which of the following proposals is the valid host range for the subnet on which the IP address 1.93.149.6/17 resides?",
          questions: [
            "1.93.127.255- 1.93.255.250",
            "1.93.128.1- 1.94.0.1",
            "1.93.128.1- 1.93.255.251",
            "1.93.128.1- 1.93.255.254",
            "1.93.128.1- 1.94.0.3"
          ],
          answer: null,
          rightAnswer: 4,
          result: null
        },
        {
          title: "What is the network address of a host with an IP address of 107.212.146.212.212/25?",
          questions: [
            "107.212.146.208",
            "107.128.0.0",
            "0.0.0.0",
            "64.0.0.0",
            "107.212.128.0",
            "107.212.146.128",
            "107.212.0.0",
            "107.208.0.0",
            "107.212.146.192"
          ],
          answer: null,
          rightAnswer: 6,
          result: null
        },
        {
          title: "Which of the following proposals is the valid host range for the subnet on which the IP address 233.249.146.36/21 resides?",
          questions: [
            "233.249.143.255-233.249.151.250",
            "233.249.144.4-233.249.152.0",
            "233.249.144.1-233.249.151.254",
            "233.249.144.6-233.249.152.1",
            "233.249.144.0-233.249.151.249"
          ],
          answer: null,
          rightAnswer: 3,
          result: null
        },
        {
          title: "Which of the following propositions is not true?",
          questions: [
            "TCP is a datagram oriented protocol",
            "TCP does not support broadcasting",
            "TCP provides extended error checking mechanisms. This is because it provides flow control and data acknowledgement",
            "Data sequencing is a TCP feature (this means that packets arrive in order in the recipient)",
            "TCP is reliable because it guarantees the delivery of data to the router of the destination",
            "TCP is comparatively slower than UDP"
          ],
          answer: null,
          rightAnswer: 1,
          result: null
        },
        {
          title: "What is the network address of a host with an IP address of 166.175.144.121/23?",
          questions: [
            "166.128.0.0",
            "166.175.144.0",
            "166.175.144.96",
            "128.0.0.0",
            "166.174.0.0",
            "166.0.0.0",
            "166.0.0.0",
            "166.175.144.120"
          ],
          answer: null,
          rightAnswer: 2,
          result: null
        },
        {
          title: "What is the maximum number of IP addresses that can be assigned to hosts on a local subnet using the 255.255.255.255.128 subnet mask?",
          questions: [
            "128",
            "60",
            "126",
            "62",
            "252",
            "258",
            "124",
            "58",
            "64"],
          answer: null,
          rightAnswer: 3,
          result: null
        },
        {
          title: "What is the maximum number of IP addresses that can be assigned to hosts on a local subnet using the 255.224.0.0 subnet mask?",
          questions: [
            "4194306",
            "4194302",
            "1048578",
            "4194300",
            "2097148",
            "4194298",
            "1048574",
            "2097150",
            "1048576"
          ],
          answer: null,
          rightAnswer: 8,
          result: null
        },
        {
          title: "Which of the following proposals is a private IP address?",
          questions: [
            "10.182.204.132",
            "116.124.85.24",
            "52.178.248.246",
            "186.183.40.79",
          ],
          answer: null,
          rightAnswer: 1,
          result: null
        },
        {
          title: "What DHCP protocol does it use at the transport layer level?",
          questions: [
            "ICMP",
            "TCP",
            "FTP",
            "UDP"
          ],
          answer: null,
          rightAnswer: 4,
          result: null
        },
        {
          title: "You have an interface on a router with the IP address of 240.19.3.205/12. Including the router interface, how many hosts can have IP addresses on the local network connected to the router interface?",
          questions: [
            "1048576",
            "2097154",
            "1048574",
            "524284",
            "1048578",
            "2097148",
            "1048572"
          ],
          answer: null,
          rightAnswer: 3,
          result: null
        },
        {
          title: "What is the broadcast address of a host with an IP address of 51.254.122.100/24?",
          questions: [
            "51.254.122.0",
            "51.254.122.1",
            "51.254.122.254",
            "51.254.122.255"
          ],
          answer: null,
          rightAnswer: 4,
          result: null
        },
        {
          title: "What is the CIDR notation of the 255.255.128.0 subnet mask?",
          questions: [
            "/8",
            "/16",
            "/9",
            "/17"
          ],
          answer: null,
          rightAnswer: 4,
          result: null
        },
        {
          title: "What is the CIDR notation of the 255.255.192.0 subnet mask?",
          questions: [
            "/5",
            "/31",
            "/18",
            "/14"
          ],
          answer: null,
          rightAnswer: 3,
          result: null
        },
        {
          title: "What are the different layers of the OSI model?",
          questions: [
            "Application - Presentation - Session - Transport - Network - Data Link - Physical",
            "Application - Mediation - Session - Transport - Network - Data Link - Physical",
            "Presentation - Session - Transport - Network - Data Link - Application - Real",
            "Relation - Transport - Session - Data Link - Mediation - Presentation - Application"],
          answer: null,
          rightAnswer: 1,
          result: null
        },
        {
          title: "What is the network address of a host with an IP address of 116.45.224.50/8?",
          questions: [
            "116.0.1.0",
            "116.0.0.0",
            "116.255.255.0",
            "116.255.255.255"
          ],
          answer: null,
          rightAnswer: 2,
          result: null
        },
        {
          title: "What is the network address of a host with an IP address of 45.195.37.187/16?",
          questions: [
            "45.194.37.187",
            "45.0.0.0",
            "45.194.0.0",
            "45.195.0.0"
          ],
          answer: null,
          rightAnswer: 4,
          result: null
        },
        {
          title: "_______ translates Internet domain names and host names into IP addresses",
          questions: [
            "Network time protocol",
            "Default routing protocol",
            "Domain name system",
            "OSI model system"
          ],
          answer: null,
          rightAnswer: 3,
          result: null
        },
        {
          title: "Which IP address class has more host addresses available by default?",
          questions: [
            "C",
            "D",
            "E",
            "F",
            "A"],
          answer: null,
          rightAnswer: 5,
          result: null
        },
        {
          title: "Which of the following propositions is not true?",
          questions: [
            "UDP is faster, simpler and more efficient than TCP",
            "UDP only has the basic error control mechanism",
            "UDP is a datagram oriented protocol",
            "UDP does not support broadcasting"
          ],
          answer: null,
          rightAnswer: 4,
          result: null
        },
        {
          title: "Which of the following propositions is not true?",
          questions: [
            "TCP is a connection-oriented protocol",
            "TCP does not support broadcasting",
            "TCP provides extended error checking mechanisms. This is because it provides flow control and data acknowledgement",
            "Data sequencing is a TCP feature (this means that packets arrive in order in the recipient)",
            "The delivery of data to the destination cannot be guaranteed in TCP",
            "TCP is reliable because it guarantees the delivery of data to the router of the destination"],
          answer: null,
          rightAnswer: 5,
          result: null
        },
        {
          title: "What is the default IP address class available?",
          questions: [
            "A AND B",
            "A",
            "C",
            "B"],
          answer: null,
          rightAnswer: 3,
          result: null
        },
        {
          title: "What is the subnet mask of /24?",
          questions: [
            "255.255.255.255",
            "255.255.255.0",
            "255.0.0.0",
            "255.255.128.0",
            "255.192.0.0",
            "255.224.0.0"],
          answer: null,
          rightAnswer: 2,
          result: null
        },
        {
          title: "Which of the following IP addresses is a private address?",
          questions: [
            "169.153.119.123",
            "24.23.102.151",
            "255.62.136.173",
            "10.166.25.20",
            "46.244.138.171",
            "27.147.158.251"
          ],
          answer: null,
          rightAnswer: 4,
          result: null
        },
        {
          title: "Which of the following IP addresses is a private address?",
          questions: [
            "222.9.230.144",
            "135.167.134.35",
            "172.32.0.5",
            "27.157.141.96",
            "172.16.0.2",
            "129.244.78.149",
            "137.223.167.235",
          ],
          answer: null,
          rightAnswer: 5,
          result: null
        },
        {
          title: "Which of the following IP addresses is a private address?",
          questions: [
            "108.246.233.231",
            "146.227.105.173",
            "59.155.254.18",
            "253.29.133.220",
            "192.168.20.253",
            "94.152.104.99"
          ],
          answer: null,
          rightAnswer: 5,
          result: null
        },
        {
          title: "What is the size of an IPV4 address?",
          questions: [
            "128 bits",
            "32 bits",
            "64 miles",
            "16 bits",
            "8 bits",
            "64 bytes",
            "128 bytes"
          ],
          answer: null,
          rightAnswer: 2,
          result: null
        },
        {
          title: "What is the size of an IPV6 address?",
          questions: [
            "128 bits",
            "32 bits",
            "64 miles",
            "16 bits",
            "8 bits",
            "64 bytes",
            "128 bytes"
          ],
          answer: null,
          rightAnswer: 1,
          result: null
        },
        {
          title: "What type of address is supported by DHCP?",
          questions: [
            "IPV4",
            "IPV6",
            "IPV4 and IPV6",
            "None of them"
          ],
          answer: null,
          rightAnswer: 3,
          result: null
        },
        {
          title: "Which protocol does Ping use?",
          questions: [
            "ARP",
            "BootP",
            "TCP",
            "ICMP"
          ],
          answer: null,
          rightAnswer: 4,
          result: null
        },
        {
          title: "Which of the following propositions is not true?",
          questions: [
            "UDP is faster, simpler and more efficient than TCP",
            "UDP provides extended error checking mechanisms. This is because it provides flow control and data acknowledgement",
            "UDP is a datagram oriented protocol",
            "UDP supports broadcasting"
          ],
          answer: null,
          rightAnswer: 2,
          result: null
        }
      ]
    }
  },
  methods:{
    checkAnswer(i){
      this.quiz[i].result = this.quiz[i].answer === this.quiz[i].rightAnswer;
      this.$refs.stepper.next();
    },
    showResult(){
      this.loading = !this.loading;
      clearInterval(this.interval);

      this.resultArr = this.quiz.filter(el => !el.result);
      this.test = !this.test;

      this.loading = !this.loading;
    },
    timer(){
      this.interval = setInterval(()=>{
        if(this.seconds === 0){
          this.seconds = 59;
          this.minutes -= 1;
        }
        this.seconds -=1;
        if(this.seconds === 0 && this.minutes === 0){
          clearInterval(this.interval);
          this.resultArr = this.quiz.filter(el => !el.result);
          this.test = !this.test;
        }
      }, 1000)
    }
  },
  created() {
    let bucket;

    for(let i = 0; i < this.quiz.length; i++){
      let randomNum = Math.floor(Math.random() * this.quiz.length);

      bucket = this.quiz[i];
      this.quiz[i] = this.quiz[randomNum];
      this.quiz[randomNum] = bucket;
    }
  },
  mounted(){
    this.timer();
  }
}
</script>
<style>
 .styling__radio{
   background: lightgray;
 }

 .q-stepper__title{
   font-family: 'Ubuntu', sans-serif;
 }
</style>
