<template>
  <b-modal
    id="detailsModal"
    class=""
    centered
    @click="$bvModal.hide('detailsModal')"
    size="lg"
    title="Details"
  >
    <section class="">
      <p class="d-flex info">
        <span class="info-name mr-3">Actors:</span>
        <span class="info-details">{{ info.Actors }}</span>
      </p>
      <p class="d-flex info">
        <span class="info-name mr-3">Awards:</span>
        <span class="info-details">{{ info.Awards }}</span>
      </p>
      <p class="d-flex info">
        <span class="info-name mr-3">Director:</span>
        <span class="info-details">{{ info.Director }}</span>
      </p>
      <p class="d-flex info">
        <span class="info-name mr-3">Genre:</span>
        <span class="info-details">{{ info.Genre }}</span>
      </p>
      <p class="d-flex info">
        <span class="info-name mr-3">Language:</span>
        <span class="info-details">{{ info.Language }}</span>
      </p>
      <p class="d-flex info">
        <span class="info-name mr-3">Plot:</span>
        <span class="info-details">{{ info.Plot }}</span>
      </p>
      <p class="d-flex info">
        <span class="info-name mr-3">Rated:</span>
        <span class="info-details">{{ info.Rated }}</span>
      </p>
      <p class="d-flex info">
        <span class="info-name mr-3">Writer:</span>
        <span class="info-details">{{ info.Writer }}</span>
      </p>
      <p class="d-flex info">
        <span class="info-name mr-3">IMDB Rating:</span>
        <span class="info-details">{{ info.imdbRating }}</span>
      </p>
    </section>
    <div slot="modal-footer">
      <button class="btn btn-default" @click="$bvModal.hide('detailsModal')">
        Close
      </button>
    </div>
  </b-modal>
</template>

<script>
export default {
  name: "DetailsModal",
  data() {
    return {};
  },
  props: {
    info: Object,
  },
  methods: {},
};
</script>

<style lang="scss">
.info {
  .info-name {
    color: #777;
    font-weight: 300;
    display: block;
    min-width: 90px;
  }
}
</style>
