<template>
  <div class="tw-grid tw-grid-cols-12 tw-w-full tw-h-screen">
    <div class="tw-col-span-6" v-if="$vuetify.breakpoint.mobile ? false : true">
      <v-img src="/ferreteria-hero.jpeg" height="100%"></v-img>
    </div>

    <div
      class="tw-col-span-12 lg:tw-col-span-6 tw-flex tw-flex-col justify-center tw-items-center"
    >
      <!--LOGO-->
      <div class="tw-mb-10">
        <v-img
          src="/logo_elancar.png"
          :width="$vuetify.breakpoint.mobile ? 150 : 250"
          :height="120"
          contain
        ></v-img>
      </div>
      <!--LOGO-->

      <!--FORMULARIO INICIO SESSION-->
      <v-form
        v-model="validado"
        lazy-validation
        ref="formulario"
        @submit="login"
      >
        <!--INPUT EMAIL-->
        <div class="tw-w-60">
          <v-text-field
            v-model="email"
            :rules="reglaNotNull"
            label="Email"
            flat
            outlined
            append-icon="mdi-email-outline"
          ></v-text-field>
        </div>
        <!--INPUT EMAIL-->

        <!--INPUT CONTRASEÑA-->
        <div class="mt-4 tw-w-60">
          <v-text-field
            v-model="password"
            type="password"
            :rules="reglaNotNull"
            label="Contraseña"
            flat
            outlined
            append-icon="mdi-lock-outline"
          ></v-text-field>
        </div>
        <!--INPUT CONTRASEÑA-->

        <!--SELECT SUCURSAL-->
        <div class="mt-4 tw-w-60">
          <v-select
            v-model="sucursal"
            :items="sucursales"
            label="Sucursal"
            outlined
            :rules="reglaNotNull"
            full-width
            :item-text="(item) => item.alias"
            :item-value="(item) => item.id"
            required
          ></v-select>
        </div>
        <!--SELECT SUCURSAL-->

        <!--BOTON SESION-->
        <div class="tw-mb-6 tw-mt-8">
          <button
            type="submit"
            class="tw-bg-neutral-900 tw-w-full tw-px-8 tw-text-white tw-py-4 tw-font-bold tw-text-sm tw-uppercase"
          >
            <span>Iniciar sesión</span>
          </button>
        </div>
        <!--BOTON SESION-->
      </v-form>
      <!--FORMULARIO INICIO SESSION-->
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  layout: "login",
  data() {
    return {
      timeout: 3000,
      validado: false,
      email: null,
      password: null,
      reglaNotNull: [(v) => !!v || "Este campo es obligatorio"],
      sucursal: null,
    };
  },
  mounted() {
    this.$store.dispatch("sucursal/loadSucursales");
  },
  methods: {
    login(e) {
      e.preventDefault();

      try {
        this.$refs.formulario.validate();

        const credenciales = {
          email: this.email,
          password: this.password,
        };

        const params = this.$route.query.redirect || "/home";


        this.$store.dispatch("sucursal/loadSucursal", this.sucursal);

        this.$store.dispatch("sucursal/setSucursal", this.sucursal);

        this.$store.dispatch("session/login", {
          credenciales: credenciales,
          params: params,
        });
      } catch (e) {}
    },
  },
  computed: {
    error() {
      return this.$store.getters["session/getError"].errorMsg;
    },
    snackbar() {
      return this.$store.getters["session/getError"].value;
    },
    sucursales() {
      return this.$store.getters["sucursal/getSucursales"];
    },
  },
};
</script>

<style></style>
