# oregano-maintenance

set -e; \
echo "🔍 Finding running Docker containers..."; \
RUNNING_CONTAINERS=$(docker ps -q); \
if [ -n "$RUNNING_CONTAINERS" ]; then \
  echo "🛑 Stopping running containers..."; \
  docker stop $RUNNING_CONTAINERS; \
else \
  echo "✅ No running containers found."; \
fi; \
echo "📦 Updating images..."; \
IMAGES=$(docker ps -a --format '{{.Image}}' | sort -u); \
for IMAGE in $IMAGES; do \
  echo "⬇️  Pulling $IMAGE..."; \
  docker pull "$IMAGE"; \
done; \
echo "🚀 Restarting containers with same options..."; \
for CID in $(docker ps -a -q); do \
  NAME=$(docker inspect --format='{{.Name}}' $CID | sed 's/^\/\(.*\)/\1/'); \
  CMD=$(docker inspect --format='{{.Path}} {{range .Args}}{{.}} {{end}}' $CID); \
  IMAGE=$(docker inspect --format='{{.Config.Image}}' $CID); \
  echo "♻️  Removing old container: $NAME"; \
  docker rm "$CID" >/dev/null; \
  echo "🚀 Starting new container: $NAME"; \
  docker run -d --name "$NAME" $IMAGE $CMD; \
done; \
echo "🎉 Done! All containers updated and restarted."
