<template>
  <div>
    <div>
      <!-- search bar -->
      <v-lazy
        v-model="isActive"
        :options="{
          threshold: 1
        }"
        transition="slide-x-reverse-transition"
        origin="top right 50"
      >
        <v-card
          color="lighten-4"
          hover="true"
          style="margin-bottom: 1rem; padding: 1rem;"
          height="90"
        >
          <v-row>
            <v-col>
              <v-text-field
                v-model="search"
                append-icon="mdi-magnify"
                label="Search"
                filled
                dense
                single-line
              ></v-text-field>
            </v-col>
            <v-col md="auto" sm="auto" xs="auto" lg="auto" xl="auto">
              <v-btn icon>
                <v-icon>mdi-heart</v-icon>
              </v-btn>
            </v-col>
            <v-col md="auto" sm="auto" xs="auto" lg="auto" xl="auto">
              <v-btn icon>
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-lazy>
    </div>
    <div>
      <v-lazy
        v-model="isActive"
        :options="{
          threshold: 1
        }"
        transition="slide-x-reverse-transition"
        origin="top right 50"
      >
        <v-card elevation="16">
          <v-card-title
            :class="this.$store.state.theme.navTheme"
            class="white--text"
          >
            <span>Record</span>
            <v-spacer></v-spacer>
          </v-card-title>
          <!-- if have record, show it -->
          <v-data-table
            :headers="recordHeaders"
            :items="recordProduct"
            :search="search"
            :items-per-page="10"
            hide-default-footer
            class="elevation-1"
            @page-count="pageCount = $event"
            :page.sync="page"
            v-if="recordProduct.length !== 0"
          >
            <template v-slot:body="{ items }">
              <tbody>
                <tr v-for="(item, index) in items" :key="item.name">
                  <td>
                    <v-tooltip top>
                      <template v-slot:activator="{ on }">
                        <v-btn
                          class="mx-2"
                          fab
                          small
                          dark
                          color="success"
                          v-on="on"
                        >
                          <v-icon>mdi-camera-image</v-icon>
                        </v-btn>
                      </template>
                      <span>
                        <v-avatar tile size="130">
                          <img
                            :src="item.img"
                          />
                        </v-avatar>
                      </span> </v-tooltip
                    >{{ item.name }}
                  </td>
                  <td>{{ item.price }}</td>
                  <td>{{ item.number }}</td>
                  <td>{{ item.total }}</td>
                  <td>{{ item.time }}</td>
                </tr>
              </tbody>
            </template>
          </v-data-table>
          <v-pagination
            v-model="page"
            :length="pageCount"
            v-if="recordProduct.length !== 0"
          ></v-pagination>
          <!-- if have no record, show it -->
          <div class="pa-4 text-center" v-if="recordProduct.length === 0">
            <v-img
              class="mb-4"
              contain
              height="128"
              src="../../../assets/icons/record.svg"
            ></v-img>
            <h3 class="title font-weight-light mb-2">Welcome to Pet Home.</h3>
            <div class="caption grey--text mb-2">Thanks for signing up!</div>
            <v-btn color="primary" to="Home">
              <v-icon left>mdi-shopping</v-icon>
              Go Shopping
            </v-btn>
          </div>
        </v-card>
      </v-lazy>
    </div>
  </div>
</template>
<script>
import { mapState, mapMutations } from "vuex";

export default {
  created() {
    // 引入 VUEX 的數據，並初始附值。
    this.recordProduct = this.recordProductItems;
    console.log(this.recordProduct)
    console.log(this.recordProductItems)
  },
  data() {
    return {
      isActive: false,
      search: "",
      number: 0,
      selected: [],
      recordHeaders: [
        {
          text: "name",
          align: "start",
          sortable: false,
          value: "name"
        },
        { text: "Price($)", value: "price" },
        { text: "number", value: "number" },
        { text: "Total($)", value: "total" },
        { text: "Time(UTF-8)", value: "time" }
      ],
      recordProduct: [],
      page: 1,
      pageCount: 0
    };
  },
  computed: {
    // get data from VUEX
    ...mapState({
      recordProductItems: state => {
        return state.product.recordProductItems;
      }
    })
  }
};
</script>
