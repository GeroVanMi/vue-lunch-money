<template>
    <div>
        <v-list-item @click="switchDialog">
            <v-list-item-icon>
                <v-icon>mdi-settings</v-icon>
            </v-list-item-icon>

            <v-list-item-content>
                <v-list-item-title>Settings</v-list-item-title>
            </v-list-item-content>
        </v-list-item>

        <v-dialog v-model="open" fullscreen transition="scroll-x-transition">
            <v-card>
                <v-app-bar fixed>
                    <v-btn color="error" @click="cancel" icon>
                        <v-icon color="error">mdi-close</v-icon>
                    </v-btn>
                    <v-toolbar-title>Einstellungen</v-toolbar-title>
                    <v-spacer/>
                    <v-btn color="success" @click="saveSettings">Save</v-btn>
                </v-app-bar>

                <v-content class="dialog-content">
                    <v-form>
                        <v-container>
                            <h2>Default Opt-In</h2>
                            <v-checkbox v-model="settings.weekdays.monday" dense label="Monday"/>
                            <v-checkbox v-model="settings.weekdays.tuesday" dense label="Tuesday"/>
                            <v-checkbox v-model="settings.weekdays.wednesday" dense label="Wednesday"/>
                            <v-checkbox v-model="settings.weekdays.thursday" dense label="Thursday"/>
                            <v-checkbox v-model="settings.weekdays.friday" dense label="Friday"/>
                        </v-container>
                        <v-divider/>
                        <v-container>
                            <h2>Sonstige </h2>
                            <v-checkbox v-model="settings.isVegetarian" dense label="Vegetarian"/>
                            <v-textarea v-model="settings.generalInfo" dense filled label="Default note on Opt-In"/>
                        </v-container>
                    </v-form>
                </v-content>
            </v-card>
        </v-dialog>
    </div>
</template>

<script>
    import {mapGetters} from 'vuex';

    export default {
        name: "settingsDialog",
        props: {

        },
        data() {
            return {
                open: false,
                settings: {
                    weekdays: {
                        monday: false,
                        tuesday: false,
                        wednesday: false,
                        thursday: false,
                        friday: false
                    },
                    isVegetarian: false,
                    generalInfo: null
                }
            }
        },
        mounted() {
            this.setDialogSettingsToStoredSettings();
        },
        computed: {
            ...mapGetters({
                storedSettings: 'getSettings'
            })
        },
        methods: {
            switchDialog() {
                this.open = !this.open;
            },
            cancel() {
                this.setDialogSettingsToStoredSettings();
                this.switchDialog();
            },
            async saveSettings() {
                await this.$store.commit('updateSettings', this.settings);
                this.switchDialog();
            },
            setDialogSettingsToStoredSettings() {
                this.settings = {
                    weekdays: {
                        monday: this.storedSettings.weekdays.monday,
                        tuesday: this.storedSettings.weekdays.tuesday,
                        wednesday: this.storedSettings.weekdays.wednesday,
                        thursday: this.storedSettings.weekdays.thursday,
                        friday: this.storedSettings.weekdays.friday
                    },
                    isVegetarian: this.storedSettings.isVegetarian,
                    generalInfo: this.storedSettings.generalInfo
                }
            }
        }
    }
</script>

<style scoped>
    .dialog-content {
        margin-top: 10px;
    }
</style>