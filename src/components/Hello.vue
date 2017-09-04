<template>
<div>
  <div>
    <div>
    <select v-model="selectedCustomer">
      <option disabled value="">Please select your customer</option>
      <option v-for="customer in customers" :value="customer">
        {{customer.title + " " + customer.firstName + " " + customer.lastName}}
      </option>
    </select>
    </div>
    <div class="card" v-if="selectedCustomer != ''">
      <h2>Customer Data</h2>
      <p>Name: {{fullName}}</p>
      <p>{{selectedCustomer.birth | date-fns}}</p>
      <p>Annual Income: {{selectedCustomer.incomeAnnual}} pounds</p>
      <p>Employment Status: {{selectedCustomer.statusEmployment}}</p>
      <p>House Numbder: {{selectedCustomer.houseNumber}}</p>
      <p>Post Code: {{selectedCustomer.postCode}}</p>
      <p id="selectedCardsCredit">Selected Cards Available Credit: {{selectedCardsCreditAvaiable}}</p>
    </div>
  </div>
  <h1 v-if="selectedCustomer != ''">Available cards:</h1>
  <div>
    <div class="card" v-for="card in cards" v-if="selectedCustomer != '' && card.ruleCheck(selectedCustomer)">
      <p>Card Name: {{ card.name }}</p>
      <p>Apr: {{ card.apr }}</p>
      <p>Balance Transfer Offer Duration: {{ card.balanceTransferOfferDuration }}</p>
      <p>Purchase Offer Duration: {{ card.purchaseOfferDuration }}</p>
      <p>Credit Available: {{ card.creditAvailable }}</p>
      <input type="checkbox" v-model="card.selected"> Select this card
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'Crazy Cards Application',
  computed: {
    fullName: function () {
      return this.selectedCustomer.title + ' ' + this.selectedCustomer.firstName + ' ' + this.selectedCustomer.lastName
    },
    selectedCards: function () {
      return this.cards.filter(function isSelected (card) {
        return card.selected
      })
    },
    selectedCardsCreditAvaiable: function () {
      var selectedCardsCreditSum = 0
      if (this.selectedCards.length > 0) {
        for (var i = 0; i < this.selectedCards.length; i++) {
          selectedCardsCreditSum += this.selectedCards[i].creditAvailable
        }
      }
      return selectedCardsCreditSum
    }
  },
  watch: {
    selectedCustomer: function () {
      for (var i = 0; i < this.cards.length; i++) {
        this.cards[i].selected = false
      }
    }
  },
  data () {
    return {
      selectedCustomer: '',
      customers: [
        {
          title: 'Mr',
          firstName: 'Ollie',
          lastName: 'Murphree',
          birth: Date('Jul 1, 1970'),
          incomeAnnual: 34000,
          statusEmployment: 'Full Time Employment',
          houseNumber: 700,
          postCode: 'BS14 9PR'
        },
        {
          title: 'Miss',
          firstName: 'Elizabeth',
          lastName: 'Edmundson',
          birth: Date('Jun 29, 1984'),
          incomeAnnual: 17000,
          statusEmployment: 'Student',
          houseNumber: 177,
          postCode: 'PH12 8UW'
        },
        {
          title: 'Mr',
          firstName: 'Trevor',
          lastName: 'Rieck',
          birth: Date('Aug 7, 1990'),
          incomeAnnual: 15000,
          statusEmployment: 'Part Time Employed',
          houseNumber: 343,
          postCode: 'TS25 2NF'
        }
      ],
      cards: [
        {
          name: 'Student Life',
          apr: 18.9,
          balanceTransferOfferDuration: 0,
          purchaseOfferDuration: 6,
          creditAvailable: 1200,
          selected: false,
          ruleCheck: function (customer) {
            return customer.statusEmployment === 'Student'
          }
        },
        {
          name: 'Anywhere',
          apr: 33.9,
          balanceTransferOfferDuration: 0,
          purchaseOfferDuration: 0,
          creditAvailable: 300,
          selected: false,
          ruleCheck: function (customer) {
            return true
          }
        },
        {
          name: 'Liquid',
          apr: 33.9,
          balanceTransferOfferDuration: 12,
          purchaseOfferDuration: 6,
          creditAvailable: 3000,
          selected: false,
          ruleCheck: function (customer) {
            return customer.incomeAnnual > 16000
          }
        }
      ]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
  text-align: center;
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
.card {
  text-align: left;
  display: inline-block;
  padding: 1rem;
  margin: 0.5rem;
  border: 1px solid #4f99b0;  
}
.selectedCustomer {
  text-align: left;
  display: inline-block;
  padding: 1rem;
  margin: 0.5rem;
  border: 1px solid #4f99b0;  
}
#selectedCardsCredit {
  font-weight: bold;
}
</style>
