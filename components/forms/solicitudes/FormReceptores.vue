<template>
  <div>
    <!-- SUBTITLE, DESCRIPTION AND SEARCH -->
    <div class="tw-flex align-center tw-mt-12">
      <div>
        <span class="tw-font-bold tw-uppercase lg:tw-text-2xl">Receptor</span>

        <div class="tw-flex">
          <span class="tw-block lg:tw-text-xl">
            Indique el receptor (vendedor) que estará a cargo de recibir
            solicitud.
          </span>
        </div>
      </div>
    </div>
    <!-- SUBTITLE, DESCRIPTION AND SEARCH -->

    <div v-if="receptores.length > 0">
      <!-- TABLE HEADERS -->
      <div class="tw-grid tw-grid-cols-12 tw-mt-8 tw-mb-4 tw-px-4">
        <div class="tw-col-span-1"></div>

        <div class="tw-col-span-6 lg:tw-col-span-5">
          <span class="tw-font-bold">Nombre</span>
        </div>

        <div class="tw-col-span-6 lg:tw-col-span-3">
          <span class="tw-font-bold">Teléfono</span>
        </div>

        <div class="tw-col-span-6 lg:tw-col-span-3">
          <span class="tw-font-bold">Correo</span>
        </div>
      </div>
      <!-- TABLE HEADERS -->

      <!-- TABLE CONTENT -->
      <div
        v-for="(receptor, index) in receptores.slice(
          itemsPerPage * page - itemsPerPage,
          itemsPerPage * page
        )"
        :key="receptor.id"
        class="tw-grid tw-grid-cols-12"
      >
        <!-- RADIO BUTTON -->
        <div
          :class="
            `${index % 2 == 0 ? 'tw-bg-white' : 'tw-bg-neutral-100'}` +
            ' tw-col-span-6 lg:tw-col-span-1 tw-px-4'
          "
        >
          <v-radio-group v-model="radioGroup" class="tw-py-0" hide-details>
            <v-radio
              :key="receptor.id"
              :value="receptor.id"
              @click="receptorSeleccionado(receptor)"
            >
            </v-radio>
          </v-radio-group>
        </div>
        <!-- RADIO BUTTON -->

        <!-- NOMBRE Y CASA MATRIZ -->
        <ReceptorTableItem
          class="tw-flex align-center tw-col-span-11"
          :receptor="receptor"
          :index="index"
          :key="receptor.id"
        />
        <!-- NOMBRE Y CASA MATRIZ -->
      </div>
      <!-- TABLE CONTENT -->

      <!-- A QUIEN CORRESPONDA -->
      <div class="tw-mt-8">
        <v-icon x-large>mdi-alert-circle-outline</v-icon>
        <span class="tw-text-xl"
          >Si no indica un receptor, por defecto, se emitirá a quién
          corresponda.</span
        >
      </div>
      <!-- <v-radio-group v-model="radioGroup" class="tw-py-0 tw-px-4" hide-details>
        <v-radio :key="0" label="A quién corresponda" @click="receptorSeleccionado(null)">
        </v-radio>
      </v-radio-group> -->
      <!-- A QUIEN CORRESPONDA -->
    </div>

    <!-- NO RESULTS -->
    <div class="tw-mt-8 tw-w-full tw-flex tw-flex-col align-center" v-else>
      <v-img
        src="/receptores_not_found.png"
        width="320"
        height="270"
        contain
      ></v-img>
      <span class="tw-font-bold tw-text-xl"
        >El proveedor no posee receptores registrados</span
      >
      <span>Por consecuencia la solicitud de emitirá a quién corresponda.</span>
    </div>
    <!-- NO RESULTS -->
  </div>
</template>

<script>
import ReceptorTableItem from "@/components/utils/table_items/ReceptorTableItem.vue";

export default {
  props: {
    receptores: {
      type: Array,
      default: () => [],
    },
    page: {
      type: Number,
      default: 1,
    },
    itemsPerPage: {
      type: Number,
      default: 10,
    },
  },
  components: {
    ReceptorTableItem,
  },
  data() {
    return {
      radioGroup: 1,
    };
  },
  methods: {
    receptorSeleccionado(receptor) {
      this.$store.dispatch(
        "carro_solicitudes/setReceptorCurrentProveedor",
        receptor
      );
    },
  },
};
</script>

<style></style>
