<template>
  <div class="mission" :class="[{ active: isActive }, normalizedStatus]">
    <div class="name">
      <h1>Mission // {{ mission.slug }}</h1>
      <h2>{{ mission.name }}</h2>
    </div>
    <div class="status" :class="normalizedStatus">
      {{ missionStatus }}
      <img :src="icon" :alt="`${normalizedStatus} mission status`" />
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  props: {
    mission: {
      type: Object,
      required: true,
    },
    selected: {
      type: String,
      required: true,
    },
  },
  computed: {
    icon() {
      // If the status isn't one of the known icon names, fall back to the generic status icon
      const allowed = ["start", "partial-success", "success", "failure"];
      if (!allowed.includes(this.normalizedStatus)) {
        return "/icons/mission-status.svg";
      }
      return `/icons/mission-${this.normalizedStatus}.svg`;
    },
    missionStatus() {
      const status = this.normalizedStatus;
      if (status === "start") return "Current\nBriefing";
      if (status === "partial-success") return "Partial\nSuccess";
      if (status === "success") return "Mission\nSuccess";
      if (status === "failure") return "Mission\nFailure";
    },
    normalizedStatus() {
      // Ensure a valid status string and map common aliases
      const s = this.mission.status || "status";
      if (s === "finished") return "success";
      return s;
    },
    isActive() {
      return this.mission.slug === this.selected;
    },
  },
};
</script>
