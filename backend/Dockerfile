FROM node:20-alpine AS builder

WORKDIR /builder
COPY . .
RUN npm pkg delete scripts.prepare \
&& npm ci --no-audit --unsafe-perm  \
&& npm run build \
&& npm ci --omit=dev && npm cache clean --force

FROM node:20-alpine
WORKDIR /app
COPY --from=builder --chown=node:node /builder/dist ./dist
COPY --from=builder --chown=node:node /builder/node_modules ./node_modules
RUN chown node:node /app
USER node

CMD ["node", "dist/index.js"]
