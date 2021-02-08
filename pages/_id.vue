<template>
  <div>
    <character-info :id="$route.params.id"/>
  </div>
</template>

<script>
import CharacterInfo from "~/components/CharacterInfo";

export default {
  components: {
    CharacterInfo
  },
  validate({params}) {
    return /^\d+$/.test(params.id)
  }
}
</script>
