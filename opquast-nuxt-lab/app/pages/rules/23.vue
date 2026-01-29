<template>
  <div class="min-h-screen bg-zinc-950 p-6">
    <div class="max-w-6xl mx-auto">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-zinc-100 mb-2">
          Gestion des Sessions Actives
        </h1>
        <p class="text-zinc-400">
          Contrôlez l'accès à votre compte depuis tous vos appareils
        </p>
      </div>

      <!-- Main Content -->
      <div class="space-y-8">
        <!-- Security Info Card -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-3">
            Sécurité de Votre Compte
          </h2>
          <p class="text-zinc-400 mb-4">
            Vous pouvez voir tous les appareils connectés à votre compte et
            déconnecter ceux que vous ne reconnaissez pas ou n'utilisez plus.
          </p>
          <div class="flex gap-4">
            <button
              @click="disconnectAllSessions"
              class="px-4 py-2 bg-red-900 text-red-100 rounded-lg hover:bg-red-800 transition-colors font-semibold"
            >
              Déconnecter Tous les Appareils
            </button>
            <button
              @click="refreshSessions"
              class="px-4 py-2 bg-zinc-700 text-zinc-100 rounded-lg hover:bg-zinc-600 transition-colors font-semibold"
            >
              Actualiser
            </button>
          </div>
        </div>

        <!-- Active Sessions List -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-2xl font-bold text-zinc-100 mb-6">
            Sessions Actives ({{ sessions.length }})
          </h2>

          <div v-if="sessions.length === 0" class="text-center py-12">
            <p class="text-zinc-400">Aucune session active</p>
          </div>

          <div v-else class="space-y-4">
            <div
              v-for="(session, index) in sessions"
              :key="index"
              class="bg-zinc-800 rounded-lg p-4 border border-zinc-700 hover:border-zinc-600 transition-colors"
            >
              <div class="flex items-start justify-between">
                <div class="flex-1">
                  <!-- Device Info -->
                  <div class="flex items-center gap-3 mb-3">
                    <div
                      class="w-10 h-10 bg-zinc-700 rounded-lg flex items-center justify-center text-zinc-100 font-bold text-lg"
                    >
                      {{ session.icon }}
                    </div>
                    <div>
                      <h3 class="text-lg font-semibold text-zinc-100">
                        {{ session.device }}
                      </h3>
                      <p class="text-sm text-zinc-400">{{ session.browser }}</p>
                    </div>
                    <div
                      v-if="session.isCurrent"
                      class="ml-auto px-3 py-1 bg-green-900 text-green-300 rounded-full text-xs font-semibold"
                    >
                      Session Actuelle
                    </div>
                  </div>

                  <!-- Session Details -->
                  <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-3">
                    <div>
                      <p class="text-xs text-zinc-500 mb-1">Localisation</p>
                      <p class="text-sm text-zinc-300">
                        {{ session.location }}
                      </p>
                    </div>
                    <div>
                      <p class="text-xs text-zinc-500 mb-1">Adresse IP</p>
                      <p class="text-sm text-zinc-300 font-mono">
                        {{ session.ip }}
                      </p>
                    </div>
                    <div>
                      <p class="text-xs text-zinc-500 mb-1">Connexion</p>
                      <p class="text-sm text-zinc-300">
                        {{ session.connectedDate }}
                      </p>
                    </div>
                    <div>
                      <p class="text-xs text-zinc-500 mb-1">Activité</p>
                      <p class="text-sm text-zinc-300">
                        {{ session.lastActivity }}
                      </p>
                    </div>
                  </div>

                  <!-- Status Badge -->
                  <div class="flex gap-2">
                    <span
                      :class="[
                        'text-xs font-semibold px-2 py-1 rounded-full',
                        session.isActive
                          ? 'bg-green-900 text-green-300'
                          : 'bg-zinc-700 text-zinc-300',
                      ]"
                    >
                      {{ session.isActive ? '✓ Actif' : 'Inactif' }}
                    </span>
                  </div>
                </div>

                <!-- Disconnect Button -->
                <button
                  v-if="!session.isCurrent"
                  @click="disconnectSession(index)"
                  class="ml-4 px-3 py-2 bg-red-900 text-red-100 rounded-lg hover:bg-red-800 transition-colors text-sm font-semibold"
                >
                  Déconnecter
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Recommendations -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Recommandations de Sécurité
          </h2>
          <div class="space-y-3">
            <div class="flex gap-3">
              <div
                class="w-6 h-6 bg-zinc-800 rounded flex items-center justify-center text-zinc-300 font-bold text-sm flex-shrink-0"
              >
                1
              </div>
              <div>
                <p class="text-zinc-100 font-semibold">
                  Vérifiez régulièrement vos sessions
                </p>
                <p class="text-sm text-zinc-400">
                  Consultez votre liste de sessions au moins une fois par mois
                </p>
              </div>
            </div>
            <div class="flex gap-3">
              <div
                class="w-6 h-6 bg-zinc-800 rounded flex items-center justify-center text-zinc-300 font-bold text-sm flex-shrink-0"
              >
                2
              </div>
              <div>
                <p class="text-zinc-100 font-semibold">
                  Déconnectez les appareils inconnus
                </p>
                <p class="text-sm text-zinc-400">
                  Si vous voyez une session que vous ne reconnaissez pas,
                  déconnectez-la immédiatement
                </p>
              </div>
            </div>
            <div class="flex gap-3">
              <div
                class="w-6 h-6 bg-zinc-800 rounded flex items-center justify-center text-zinc-300 font-bold text-sm flex-shrink-0"
              >
                3
              </div>
              <div>
                <p class="text-zinc-100 font-semibold">
                  Utilisez des mots de passe forts
                </p>
                <p class="text-sm text-zinc-400">
                  Combinez majuscules, minuscules, chiffres et caractères
                  spéciaux
                </p>
              </div>
            </div>
            <div class="flex gap-3">
              <div
                class="w-6 h-6 bg-zinc-800 rounded flex items-center justify-center text-zinc-300 font-bold text-sm flex-shrink-0"
              >
                4
              </div>
              <div>
                <p class="text-zinc-100 font-semibold">
                  Activez l'authentification à deux facteurs
                </p>
                <p class="text-sm text-zinc-400">
                  Ajoutez une couche supplémentaire de sécurité à votre compte
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Confirmation Modal -->
    <div
      v-if="showConfirmModal"
      class="fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-50"
    >
      <div
        class="bg-zinc-900 rounded-lg p-6 max-w-sm w-full border border-zinc-800"
      >
        <h3 class="text-xl font-bold text-zinc-100 mb-3">
          Confirmer la Déconnexion
        </h3>
        <p class="text-zinc-400 mb-6">{{ confirmMessage }}</p>
        <div class="flex gap-3">
          <button
            @click="showConfirmModal = false"
            class="flex-1 px-4 py-2 bg-zinc-700 text-zinc-100 rounded-lg hover:bg-zinc-600 transition-colors font-semibold"
          >
            Annuler
          </button>
          <button
            @click="confirmDisconnect"
            class="flex-1 px-4 py-2 bg-red-900 text-red-100 rounded-lg hover:bg-red-800 transition-colors font-semibold"
          >
            Déconnecter
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SessionManagement',
  data() {
    return {
      sessions: [],
      showConfirmModal: false,
      confirmMessage: '',
      pendingAction: null,
      pendingIndex: null,
    }
  },
  methods: {
    disconnectSession(index) {
      this.pendingAction = 'single'
      this.pendingIndex = index
      this.confirmMessage = `Êtes-vous sûr de vouloir déconnecter "${this.sessions[index].device}" ?`
      this.showConfirmModal = true
    },
    disconnectAllSessions() {
      this.pendingAction = 'all'
      this.confirmMessage =
        'Êtes-vous sûr de vouloir déconnecter tous les appareils ? Vous devrez vous reconnecter.'
      this.showConfirmModal = true
    },
    confirmDisconnect() {
      if (this.pendingAction === 'single') {
        this.sessions.splice(this.pendingIndex, 1)
      } else if (this.pendingAction === 'all') {
        this.sessions = []
      }
      this.showConfirmModal = false
      this.pendingAction = null
      this.pendingIndex = null
    },
    refreshSessions() {
      // Simulates a refresh - could fetch from API in real app
      this.sessions = this.generateSessions()
    },
    generateSessions() {
      return [
        {
          device: 'MacBook Pro - Safari',
          browser: 'Safari sur macOS',
          icon: '💻',
          location: 'Paris, France',
          ip: '192.168.1.45',
          connectedDate: '29 Jan 2026',
          lastActivity: "À l'instant",
          isActive: true,
          isCurrent: true,
        },
        {
          device: 'iPhone 14 - Safari',
          browser: 'Safari sur iOS',
          icon: '📱',
          location: 'Paris, France',
          ip: '92.184.98.201',
          connectedDate: '28 Jan 2026',
          lastActivity: 'Il y a 2 heures',
          isActive: true,
          isCurrent: false,
        },
        {
          device: 'iPad Pro - Chrome',
          browser: 'Chrome sur iPadOS',
          icon: '📱',
          location: 'Lyon, France',
          ip: '88.165.45.89',
          connectedDate: '25 Jan 2026',
          lastActivity: 'Il y a 1 jour',
          isActive: false,
          isCurrent: false,
        },
        {
          device: 'Windows PC - Chrome',
          browser: 'Chrome sur Windows',
          icon: '🖥️',
          location: 'Toulouse, France',
          ip: '78.234.56.123',
          connectedDate: '20 Jan 2026',
          lastActivity: 'Il y a 5 jours',
          isActive: false,
          isCurrent: false,
        },
      ]
    },
  },
  mounted() {
    this.sessions = this.generateSessions()
  },
}
</script>

<style scoped>
/* Additional custom styles if needed */
</style>
