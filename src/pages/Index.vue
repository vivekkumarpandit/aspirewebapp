<template>
  <div id="isbrowsersupport" class="bg-red">
    This Web App does not supported on desktop .Please open in mobile view
  </div>
  <div id="app" class="headercolor">
    <q-header bordered class="text-primary headercolor" style="z-index: 0">
      <q-toolbar>
        <q-toolbar-title style="font-size: 15px; color: white"
          >Account Balance
        </q-toolbar-title>
        <q-toolbar-item style="font-size: 11px; color: white"> </q-toolbar-item>
      </q-toolbar>
      <div class="row">
        <div class="col-md-4">
          <q-btn
            dense
            color="white"
            class="bg-green"
            style="margin-left: 10px; width: 35px; height: 10px"
            label="$$"
          />&nbsp;&nbsp;
          <label style="font-size: 20px; font-weight: bold; color: white">{{
            balance
          }}</label>
        </div>
        <div class="col-md-6"></div>
        <div class="col-md-2">
          <q-btn
            flat
            icon="add"
            class="btnstyeforaddnewcard"
            label="New Card"
            @click="AddNewCardFunc"
          ></q-btn>
        </div>

        <div class="headercolor" style="width: 100%">
          <q-tabs
            v-model="cardtab"
            active-color="white"
            indicator-color="blue"
            class="text-grey"
            flat
          >
            <q-tab
              name="Mydebitcards"
              label="My debit cards"
              class="headercolor"
            />
            <q-tab
              name="Allcompanycards"
              label="All company cards"
              class="headercolor"
              :disable="true"
            />
          </q-tabs>

          <q-tab-panels v-model="cardtab" animated>
            <q-tab-panel name="Mydebitcards" class="headercolor">
              <q-carousel
                v-model="slide"
                transition-prev="slide-right"
                transition-next="slide-left"
                swipeable
                animated
                control-color="green"
                navigation
                padding
                height="300px"
              >
                <q-carousel-slide
                  :name="index + 1"
                  class="column no-wrap carouselstyle"
                  v-for="(crd, index) in CardDetails"
                  :key="crd.id"
                >
                  <div style="float: right">
                    <q-btn
                      color="grey-4"
                      dense
                      text-color="green"
                      unelevated
                      label="Show card number"
                      icon="book"
                      style="width: 180px; float: right"
                    />
                  </div>

                  <q-markup-table flat bordered class="fit cardstyleandcolor">
                    <tr>
                      <td colspan="1"></td>
                      <td colspan="3">
                        <img
                          src="../statics/aspirelogo.png"
                          style="width: 100px; height: 50px; float: right"
                        />
                      </td>
                    </tr>
                    <tr>
                      <td colspan="2">
                        <label style="font-size: 20px; font-weight: bold">{{
                          crd.name
                        }}</label>
                      </td>
                      <td colspan="2"></td>
                    </tr>
                    <tr>
                      <td colspan="4">
                        <label style="font-size: 17px; font-weight: bold">
                          ****&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                          ****&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ****
                          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {{ crd.cardnumber4 }}
                        </label>
                      </td>
                    </tr>
                    <tr>
                      <td>
                        <label style="font-size: 15px; font-weight: bold"
                          >{{ crd.day }} :{{ crd.Expirydate }}</label
                        >
                      </td>
                      <td>
                        <label style="font-size: 15px; font-weight: bold"
                          >CVV : ***</label
                        >
                      </td>
                      <td colspan="2"></td>
                    </tr>
                    <tr>
                      <td colspan="2"></td>
                      <td colspan="2">
                        <label style="font-size: 25px; font-weight: bold"
                          ><i>VISA</i></label
                        >
                      </td>
                    </tr>
                  </q-markup-table>
                </q-carousel-slide>
              </q-carousel>
            </q-tab-panel>

            <q-tab-panel name="Allcompanycards" class="headercolor">
              <allcompanydebitcardtab />
            </q-tab-panel>
          </q-tab-panels>
        </div>
      </div>
    </q-header>

    <div style="z-index: 1" class="headercolor">
      <q-markup-table
        class="word-wrap text-center"
        style="border-radius: 20px 20px 0px 0px"
      >
        <tr>
          <td>
            <q-btn
              dense
              round
              color="primary"
              icon="local_activity"
              @click="ClickForUnfreezeandFreezefunc"
            /><br />
            <label class="fontsizeforbtnlabel">
              {{ Freezecardlabel }} <br />
              Card
            </label>
          </td>
          <td>
            <q-btn dense round color="primary" icon="speed" /><br />
            <label class="fontsizeforbtnlabel"
              >Set spend<br />
              limit</label
            >
          </td>
          <td>
            <q-btn dense round color="primary" icon="event" /><br />
            <label class="fontsizeforbtnlabel"
              >Add to<br />
              Gpay</label
            >
          </td>
          <td>
            <q-btn dense round color="primary" icon="refresh" /><br />
            <label class="fontsizeforbtnlabel"
              >Replace<br />
              card</label
            >
          </td>
          <td>
            <q-btn
              dense
              round
              color="primary"
              icon="delete"
              @click="CancelCard"
            /><br />
            <label class="fontsizeforbtnlabel"
              >Cancel<br />
              card</label
            >
          </td>
        </tr>
      </q-markup-table>
      <div>
        <q-list class="rounded-borders">
          <q-expansion-item
            header-class="bg-white"
            icon="card_giftcard"
            label="Card Details"
          >
            <q-card>
              <q-card-section> </q-card-section>
            </q-card>
          </q-expansion-item>

          <q-expansion-item
            icon="local_activity"
            header-class="bg-white"
            label="Recent Transection"
            default-opened
          >
            <q-card>
              <q-card-section>
                <q-list
                  bordered
                  separator
                  v-for="transecitem in RecentTransectionList"
                  :key="transecitem.id"
                >
                  <q-item clickable>
                    <q-item-section top avatar>
                      <q-avatar>
                        <q-icon name="flight" />
                      </q-avatar>
                    </q-item-section>

                    <q-item-section>
                      <q-item-label>{{ transecitem.Transecname }}</q-item-label>
                      <q-item-label caption>{{
                        transecitem.Date
                      }}</q-item-label>
                      <q-item-label
                        ><q-icon name="local_activity" />{{
                          transecitem.Status
                        }}</q-item-label
                      >
                    </q-item-section>

                    <q-item-section side top>
                      <q-badge label="$$" class="bg-green" /><label>{{
                        transecitem.Ammount
                      }}</label>
                    </q-item-section>
                  </q-item>
                </q-list>
                <q-item clickable>
                  <q-item-section>
                    <q-item-label>View all card transection </q-item-label>
                  </q-item-section>
                </q-item>
              </q-card-section>
            </q-card>
          </q-expansion-item>
        </q-list>
      </div>
    </div>

    <q-footer
      bordered
      class="bg-white text-primary"
      style="bottom: 0; position: fixed"
    >
      <q-tabs
        no-caps
        active-color="green"
        indicator-color="transparent"
        class="text-grey"
        v-model="tab"
      >
        <q-tab dense name="Home" label="Home" icon="home" :disable="true" />
        <q-tab dense name="Cards" label="Cards" icon="local_activity"> </q-tab>
        <q-tab
          dense
          name="Payments"
          label="Payments"
          icon="money"
          :disable="true"
        />
        <q-tab
          dense
          name="Credit"
          label="Credit"
          icon="card_giftcard"
          :disable="true"
        />
        <q-tab
          dense
          name="Profile"
          label="Profile"
          icon="local_florist"
          :disable="true"
        ></q-tab>
      </q-tabs>
    </q-footer>
  </div>
  <q-dialog v-model="Addnewcard" persistent>
    <q-card>
      <q-bar>
        <div>Add New Card</div>

        <q-space />

        <q-btn dense flat icon="close" v-close-popup>
          <q-tooltip>Close</q-tooltip>
        </q-btn>
      </q-bar>

      <q-card-section>
        <div>
          <label>Card Name</label>
          <q-input dense outlined v-model="Cardnamemodel" />
          <label>Card Number</label>
          <q-input
            dense
            outlined
            v-model="Cardnumbermodel"
            disable
            readonly
            mask="####-####-####-####"
          />
          <label>Expiry Date</label>
          <q-input filled dense v-model="Expirydate" disable readonly>
            <template v-slot:prepend>
              <q-icon name="event" class="cursor-pointer">
                <q-popup-proxy
                  cover
                  transition-show="scale"
                  transition-hide="scale"
                >
                  <q-date dense v-model="Expirydate" mask="YYYY-MM-DD HH:mm">
                    <div class="row items-center justify-end">
                      <q-btn v-close-popup label="Close" color="primary" flat />
                    </div>
                  </q-date>
                </q-popup-proxy>
              </q-icon>
            </template>

            <template v-slot:append>
              <q-icon name="access_time" class="cursor-pointer">
                <q-popup-proxy
                  cover
                  transition-show="scale"
                  transition-hide="scale"
                >
                  <q-time
                    v-model="Expirydate"
                    mask="YYYY-MM-DD HH:mm"
                    format24h
                  >
                    <div class="row items-center justify-end">
                      <q-btn v-close-popup label="Close" color="primary" flat />
                    </div>
                  </q-time>
                </q-popup-proxy>
              </q-icon>
            </template>
          </q-input>

          <q-btn
            color="primary"
            @click="SaveNewCard"
            label="Submit"
            style="margin-top: 5px"
          />
        </div>
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script>
import { ref } from "vue";
import { fasFont } from "@quasar/extras/fontawesome-v5";
import "@quasar/extras/fontawesome-v5";
import allcompanydebitcardtab from "components/AllCompanyDebitCardView.vue";
import { date } from "quasar";

export default {
  components: {
    allcompanydebitcardtab,
  },
  el: "#app",
  setup() {
    return {
      Freezecardlabel: "Freeze",
      Cardnamemodel: "",
      Expirydate: ref("2023-02-01 12:44"),
      Cardnumbermodel: "",
      Addnewcard: ref(false),
      cardtab: ref("Mydebitcards"),
      tab: ref("Cards"),
      balance: 20000,
      slide: ref(1),
      CardDetails: [
        {
          id: 1,
          name: "Mark Henry",
          cardnumber1: "2343",
          cardnumber2: "8888",
          cardnumber3: "9999",
          cardnumber4: "0000",
          day: "Sat",
          Expirydate: "11/23",
          CVV: "655",
        },
        {
          id: 2,
          name: "Mark Henry",
          cardnumber1: "2343",
          cardnumber2: "8888",
          cardnumber3: "9999",
          cardnumber4: "0000",
          day: "Sat",
          Expirydate: "11/23",
          CVV: "655",
        },
      ],
      RecentTransectionList: [
        {
          id: 1,
          Transecname: "Hemlays",
          Date: "10/10/2021",
          Ammount: 200,
          Status: "Refund on debit card",
          type: "Refund",
        },
        {
          id: 2,
          Transecname: "Hemlays",
          Date: "11/10/2021",
          Ammount: 100,
          Status: "Charged on debit card",
          type: "Charged",
        },
        {
          id: 3,
          Transecname: "Hemlays",
          Date: "12/10/2021",
          Ammount: 250,
          Status: "Charged on debit card",
          type: "Charged",
        },
        {
          id: 4,
          Transecname: "Hemlays",
          Date: "13/10/2021",
          Ammount: 600,
          Status: "Refund on debit card",
          type: "refund",
        },
      ],
    };
  },

  methods: {
    SaveNewCard: function () {
      if (this.Cardnamemodel == "") {
        alert("Please enter card number");
        return false;
      } else {
        var currenobj = this;
        console.log(currenobj.Cardnumbermodel);

        const d = new Date(currenobj.Expirydate);
        let day = this.GetDayString(d.getDay());

        let year = d.getYear();
        let month = d.getMonth();

        var data = {
          id: currenobj.CardDetails.length + 1,
          name: currenobj.Cardnamemodel,
          cardnumber1: currenobj.Cardnumbermodel.slice(0, 4),
          cardnumber2: currenobj.Cardnumbermodel.slice(4, 8),
          cardnumber3: currenobj.Cardnumbermodel.slice(8, 12),
          cardnumber4: currenobj.Cardnumbermodel.slice(12, 16),
          day: day,
          Expirydate: month + "/" + year,
          CVV: "",
        };
        //this.$set(this.CardDetails, this.CardDetails.length + 1, data);
        this.CardDetails = data;
        alert("Card saved");
        this.Addnewcard = false;
      }
    },
    GetDayString: function (day) {
      if (day == 0) {
        return "Sun";
      } else if (day == 1) {
        return "Mon";
      } else if (day == 2) {
        return "Tue";
      } else if (day == 3) {
        return "Wed";
      } else if (day == 4) {
        return "Thru";
      } else if (day == 5) {
        return "Fri";
      } else if (day == 5) {
        return "Sat";
      }
    },
    AddNewCardFunc: function () {
      this.Addnewcard = true;
      var min = 1111111111111111;
      var max = 999999999999999;
      min = Math.ceil(min);
      max = Math.floor(max);
      this.Cardnumbermodel = (
        Math.floor(Math.random() * (max - min + 1)) + min
      ).toString();
      this.Expirydate = this.randomDate(new Date(), new Date(2022, 0, 1));
    },
    randomDate: function (start, end) {
      return new Date(
        start.getTime() + Math.random() * (end.getTime() - start.getTime())
      );
    },
    ClickForUnfreezeandFreezefunc: function () {
      if (this.Freezecardlabel == "Freeze") {
        this.Freezecardlabel = "UnFreeze";
      } else {
        this.Freezecardlabel = "Freeze";
      }
    },
    CancelCard: function () {
      //this.CardDetails = [];
      alert("alert event");
    },
  },

  mounted() {},
};
</script>
<style>
.fontsizeforbtnlabel {
  font-size: 11px;
}
.cardstyleandcolor {
  background-color: rgb(1, 209, 103);
  color: white;
  line-height: 100%;
  border-radius: 10px 10px 10px 10px;
}
.carouselstyle {
  background-color: rgb(12, 54, 90);
  border: 0px;
}
.btnstyeforaddnewcard {
  float: right;
  color: rgb(24, 130, 172);
  margin-left: 100px;
}
.headercolor {
  background-color: rgb(12, 54, 90);
}
@media screen and (min-width: 500px) {
  #app {
    display: none;
  }
  #isbrowsersupport {
    display: block;
  }
}

/* On screens that are 600px or less, set the background color to olive */
@media screen and (max-width: 500px) {
  #app {
    display: block;
  }
  #isbrowsersupport {
    display: none;
  }
}
</style>
