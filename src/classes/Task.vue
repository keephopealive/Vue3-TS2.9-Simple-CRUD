<script lang="ts">
export default class Task {
    constructor(
        public title: string =  '',
        public description: string = '',
        public completed: boolean = true,
    ){}
}
</script>