<template>
  <div class="row q-col-gutter-sm q-py-sm">
    <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12">
      <q-card class="text-grey-8">
        <q-card-section class="q-pa-none">
          <q-table
            class="no-shadow"
            :data="users"
            title="რეგისტრირებულთა სია"
            :hide-header="mode === 'grid'"
            :columns="columns"
            row-key="privatenumber"
            :filter="filter"
            :pagination.sync="pagination"
          >
            <template v-slot:top-right="props">
              <q-input
                borderless
                dense
                debounce="300"
                v-model="filter"
                placeholder="Search"
              >
                <template v-slot:append>
                  <q-icon name="search" />
                </template>
              </q-input>

              <q-btn
                flat
                round
                dense
                :icon="props.inFullscreen ? 'fullscreen_exit' : 'fullscreen'"
                @click="props.toggleFullscreen"
                v-if="mode === 'list'"
                class="q-px-sm"
              >
                <q-tooltip :disable="$q.platform.is.mobile" v-close-popup
                  >{{
                    props.inFullscreen ? "Exit Fullscreen" : "Toggle Fullscreen"
                  }}
                </q-tooltip>
              </q-btn>

              <q-btn
                color="green"
                icon-right="archive"
                label="Export to csv"
                no-caps
                @click="exportTable"
              />
            </template>
          </q-table>
        </q-card-section>
      </q-card>
    </div>
  </div>
</template>
<script>
import { exportFile } from "quasar";
import { mapState, mapMutations } from "vuex";
export default {
  name: "dashboard",
 methods: {
    ...mapMutations(["incrementCounter"]),
  },
  computed: {
    ...mapState(["storeCounter"]),
  },
  mounted() {
    this.$store.dispatch("fetchUsers").then(() => {
      this.users = this.$store.state.users;
    });
  },
  data() {
    return {
      users:[],
      filter: "",
      mode: "list",
      columns: [
        {
          name: "name",
          align: "left",
          label: "სახელი",
          field: "name",
          sortable: true
        },
        {
          name: "lastname",
          align: "left",
          label: "გვარი",
          field: "lastname",
          sortable: true
        },
        {
          name: "privatenumber",
          required: true,
          label: "პირადი ნომერი",
          align: "left",
          field: "privatenumber",
          sortable: true
        },
        {
          name: "dateofbirth",
          align: "right",
          label: "დაბადების თარიღი",
          field: "dateofbirth",
          sortable: true
        },
        {
          name: "gender",
          align: "right",
          label: "სქესი",
          field: "gender",
          sortable: true
        }
      ],
      data: [
        {
          name: "კონსტანტინე",
          lastname: "ჯანჯღავა",
          dateofbirth: "22-03-1993",
          privatenumber: "87321445824",
          gender: "მამრობითი"
        },
        {
          name: "გიორგი",
          lastname: "ლაბაძე",
          dateofbirth: "22-03-1993",
          privatenumber: "28494012341",
          gender: "მამრობითი"
        },
        {
          name: "ალექსანდრე",
          lastname: "ჯიქია",
          dateofbirth: "22-03-1993",
          privatenumber: "94738456012",
          gender: "მამრობითი"
        },
        {
          name: "უჩა",
          lastname: "მაჭაიძე",
          dateofbirth: "02-06-1993",
          privatenumber: "18494012341",
          gender: "მამრობითი"
        },
        {
          name: "ლევან",
          lastname: "წერეთელი",
          dateofbirth: "22-12-1990",
          privatenumber: "38494012341",
          gender: "მამრობითი"
        },
        {
          name: "გიორგი",
          lastname: "ბარათაშვილი",
          dateofbirth: "12-01-1973",
          privatenumber: "41111111111",
          gender: "მამრობითი"
        },
        {
          name: "რეზო",
          lastname: "როგავა",
          dateofbirth: "12-03-1991",
          privatenumber: "68494012341",
          gender: "მამრობითი"
        }
      ],
      pagination: {
        rowsPerPage: 10
      }
    };
  },
  methods: {
    exportTable() {
      // naive encoding to csv format
      const content = [this.columns.map(col => wrapCsvValue(col.label))]
        .concat(
          this.data.map(row =>
            this.columns
              .map(col =>
                wrapCsvValue(
                  typeof col.field === "function"
                    ? col.field(row)
                    : row[col.field === void 0 ? col.name : col.field],
                  col.format
                )
              )
              .join(",")
          )
        )
        .join("\r\n");

      const status = exportFile("table-export.csv", content, "text/csv");

      if (status !== true) {
        this.$q.notify({
          message: "Browser denied file download...",
          color: "negative",
          icon: "warning"
        });
      }
    }
  }
};
</script>
<style scoped>
</style>